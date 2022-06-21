---
layout: post
title:  "Returning multiple outputs from a TensorRT model inference"
date:   2022-06-06 13:37:00 +0200
---

[TensorRT](https://developer.nvidia.com/tensorrt) is the de facto SDK for optimizing neural network inference on [Nvidia devices](https://developer.nvidia.com/cuda-gpus). There are some great resources out there for using TensorRT, but there is one question I did not find answer anywhere:

# How to return multiple output bindings from a TensorRT model

Input and output bindings in TensorRT correspond to input and output layers in neural networks. It's easy to run inference on a model with a single input and output binding. But imagine the following scenario: you have a neural network with a single input and multiple outputs. How would you get all the outputs for an input in one pass? We are going to find that out in this article. Let's start from scratch.

## The model

We are using the ELG model from [swook/GazeML](https://github.com/swook/GazeML) repository as our example. Please use [my fork](https://github.com/Hyrtsi/GazeML) of the repository that includes some [bugfixes](https://github.com/swook/GazeML/issues/90). Let's start off by cloning the repo.

```
git clone git@github.com:Hyrtsi/GazeML.git
```

We need Python 3.7 to run this code. Download it [here](https://www.python.org/downloads/). Just take one like [this](https://www.python.org/downloads/release/python-3713/), take either gzip or tarball and follow [these instructions](https://opensource.com/article/20/4/install-python-linux). Verify your installation by typing `python3.7` in your terminal. Something like this should show up

```
$ python3.7
Python 3.7.12 (default, Mar 11 2022, 12:03:27) 
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

After that you need to create a python virtual environment ([venv](https://docs.python.org/3/tutorial/venv.html)) to avoid messing up your system. You can use [conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html) instead if you prefer that. Discussion [here](https://www.quora.com/When-and-why-should-one-use-conda-pip-or-venv-What-are-the-differences-between-them) and [here](https://stackoverflow.com/questions/34398676/does-conda-replace-the-need-for-virtualenv).

Run the following inside `GazeML` repository root folder:
```
python3.7 -m venv .venv
source .venv/bin/activate
```

This command created a folder named `.venv` that has all the package information for your virtual environment. If you want to exit the venv just type `deactivate` at any time. Now let's install all necessary packages using `pip`:

```
pip install --upgrade pip
pip install cython
pip install scipy
python3 setup.py install
pip install tensorflow==1.14
pip install tensorflow-gpu==1.14
```

I tried using Tensorflow 2.x with this repository but it didn't work. I also tried using `tensorflow==1.15` but it didn't work either. Be careful with the package versions.

### Download trained weights

```
bash get_trained_weights.bash
```

### Run the demo

```
cd src
python3 elg_demo.py
```

You should see your face with your webcam, the eyelids, iris and gaze direction.

![GazeML output]({{ site.baseurl}}/assets/gaze.png)

### Convert the model to ONNX

Next we have to convert the model from TensorFlow file format to `.onnx` and then from `.onnx` to `.engine`. [ONNX](https://onnx.ai/) is a nexus between different pretrained neural network file formats. It lets us convert the TensorFlow model to TensorRT.

Now that we have downloaded the weights we need to use this amazing opensource tool, [tf2onnx](https://github.com/onnx/tensorflow-onnx), to convert the model.

```
pip install -U tf2onnx
```

If you ran the code you should have a folder named `tmp` that has the saved-model file: `saved_model.pb`.

You can convert that file to `.onnx` like so:

```
python3 -m tf2onnx.convert --saved-model ./tmp --output gazeml.onnx
```

For most of your needs that should be enough. You can add `--opset <opset>` for example `--opset 10` if you want to target a specific opset. You can also add `--target tensorrt` or similar. Check the [here](https://github.com/onnx/tensorflow-onnx#cli-reference) for more flags if you need them.

There will be a lot of prints in the console. Check for any errors. If everything went well you should have now an `.onnx` file of the ELG model.

### Convert the model to TensorRT .engine

Now we are stepping into TensorRT world. It means we need to install some dependencies.

#### Check that you have a [CUDA capable GPU](https://developer.nvidia.com/cuda-gpus).

#### Install CUDA

Find the steps for your system [here](https://developer.nvidia.com/cuda-downloads). For Ubuntu 20.04 and CUDA 11.7:

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.7.0/local_installers/cuda-repo-ubuntu2004-11-7-local_11.7.0-515.43.04-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-7-local_11.7.0-515.43.04-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2004-11-7-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```

Note: this depends on `nvidia-driver-515`. If you have an older version replace the last step in the command above with `sudo aptitude install cuda` to resolve conflicts or do this:

```
sudo apt-get remove --purge nvidia-*
sudo apt-get remove --purge *nvidia*
```

This removes all `apt` packages related to nvidia. They will be reinstalled upon `cuda` installation.

#### Reboot your PC 

```
sudo reboot now
```

and check the output of `nvidia-smi` to verify your installation.

```
nvidia-smi
```

It should look something like this

```
$nvidia-smi
Mon Jun  6 22:08:22 2022       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 510.60.02    Driver Version: 510.60.02    CUDA Version: 11.6     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0  On |                  N/A |
| N/A   58C    P8    17W /  N/A |    874MiB / 16384MiB |     26%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

```


#### Install TensorRT

Follow [these steps](https://docs.nvidia.com/deeplearning/tensorrt/quick-start-guide/index.html#install). You need to register to Nvidia portal. It's free.

Download TensorRT repo [here](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#downloading).

For TensorRT 8.2.3.0, Ubuntu 20.04 and CUDA >= 11.4 the installation goes like this:
```
sudo dpkg -i nv-tensorrt-repo-ubuntu2004-cuda11.4-trt8.2.3.0-ga-20220113_1-1_amd64.deb
sudo apt-key add /var/nv-tensorrt-repo-ubuntu2004-cuda11.4-trt8.2.3.0-ga-20220113/*.pub
sudo apt-get update
sudo apt-get install tensorrt
```

You should be able to convert the model next using [trtexec](https://docs.nvidia.com/deeplearning/tensorrt/developer-guide/index.html#trtexec), TensorRT's own engine converter tool

```
/usr/src/tensorrt/bin/trtexec --onnx=gazeml.onnx --saveEngine=gazeml.engine --fp16 --workspace=3000 --buildOnly
```

Congratulations. Now you have the model. Let's use it for something cool next.

## Loading TensorRT .engine in c++

To be continued...

We will base our code to [this](https://github.com/noahmr/yolov5-tensorrt) amazing repository.

