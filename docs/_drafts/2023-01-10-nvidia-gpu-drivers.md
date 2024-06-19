---
layout: post
title:  "Installing Nvidia drivers, CUDA, cuDNN and TensorRT on Linux made easy"
date:   2023-01-15 13:37:00 +0200
tags: nvidia gpu drivers
author: Eljas Hyyrynen (hyrtsi)
---

Using Nvidia GPU on Linux isn't always easy.

I have spent my fair share of time installing, removing and reinstalling the drivers. However, most of the time things just work.
I'll walk you through what I have learned.

# Nvidia drivers for Machine Learning Engineers

The basis of everything is the package `nvidia-driver-xyz`

You can see the list of possible packages like this:

```
sudo apt-cache search nvidia-driver-
```

The drivers you're interested are probably these:

```
nvidia-driver-435 - Transitional package for nvidia-driver-455
nvidia-driver-440 - Transitional package for nvidia-driver-450
nvidia-driver-450-server - NVIDIA Server Driver metapackage
nvidia-driver-470 - NVIDIA driver metapackage
nvidia-driver-470-server - NVIDIA Server Driver metapackage
nvidia-driver-510 - NVIDIA driver metapackage
nvidia-driver-510-server - Transitional package for nvidia-driver-515-server
nvidia-driver-515 - NVIDIA driver metapackage
nvidia-driver-515-open - NVIDIA driver (open kernel) metapackage
nvidia-driver-515-server - NVIDIA Server Driver metapackage
nvidia-driver-520-open - Transitional package for nvidia-driver-525
nvidia-driver-525 - NVIDIA driver metapackage
nvidia-driver-525-open - NVIDIA driver (open kernel) metapackage
nvidia-driver-525-server - NVIDIA Server Driver metapackage
```

You can check the packages you have installed like this

```
sudo dpkg -l | grep nvidia
```

If you don't already know, `dpkg -l` shows all installed packages. Piping that to grep searches a keyword, in our case `nvidia` and thus shows all installed packages that have `nvidia` somewhere in their name.

You can check the nvidia packages you already have installed. If you already have some packages installed please remove everything before starting unless you know what you're doing.

You need to start from scratch. Having conflicting packages makes your life worse. I have never messed my system totally up by doing this but it would be clever to make a backup of everything precious before continuing. If you lose your video after removing packages you can still access your system, install packages and backup files by booting to troubleshooting mode.

Let's start by removing old packages:

```
sudo apt remove --purge nvidia-driver-* 
sudo apt remove --purge nvidia-*
sudo apt remove --purge cuda
sudo apt remove --purge cudnn
sudo apt remove --purge tensorrt
```

After you do
```
sudo dpkg -l | grep 'nvidia\|cuda\|cudnn\|tensorrt'
```

you should get nothing as an output. If you do, please remove those packages one by one.

Then run
```
sudo apt autoremove
```

to remove packages that were dependencies of the removed packages but are no longer needed.

Note that you have just uninstalled your Nvidia GPU drivers and related CUDA packages.
Your other display might stop working temporarily. All packages that depend on `nvidia-drivers-xyz` and `cuda` and `cudnn` and `tensorrt` might stop working until you reinstall the packages. No need to be afraid.

---

## Install nvidia driver

```
sudo apt install nvidia-driver-525
```

Nowadays you don't have to install nvidia drivers if you install CUDA as the CUDA packages will contain all dependencies. You just need a CUDA-capable GPU and a suitable compiler (for example a fairly recent version of gcc). I have built with gcc-8, gcc-9, gcc-10 and gcc-11.

Please restart your PC after this operation

```
sudo reboot now
```

and then check the output of `nvidia-smi`.

If it does not work, reboot your PC again. If that does not work, uninstall ALL nvidia-related packages and reinstall the graphics drivers.

## Install CUDA

I usually use the deb package to install CUDA. I just follow the instructions and it works.

There are no notable pre-installation steps that need to be done usually when doing this.

However, these days CUDA packages try to force you to use the latest nvidia drivers. If you want to use your own nvidia driver version use the runfiles. 

The nvidia driver vs CUDA support matrix is here: (todo).

You need to disable Nouveau before installing the runfile. The steps to do that are here:

```
sudo service lightdm stop
```

this will shut down your graphical user interface. If it's your first time doing it, don't be scared. Have a device with internet connection and a browser in hand because you might run into trouble.

Press ctrl + alt + F1 (or F2 or F3 or any function key) to pick a terminal session.

Log in using your username and password. That will take you to home.

Following [this](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#id13) guide, disable Nouveau.

Create a file at /etc/modprobe.d/blacklist-nouveau.conf with the following contents:
```
blacklist nouveau
options nouveau modeset=0
```
Save and exit. Then run:

```
sudo update-initramfs -u
``` 

Navigate to where you had the runfile and then execute it. For example:

```
sudo sh cuda_11.6.2_510.47.03_linux.run
```

Don't worry if it takes a while for anything to happen after you enter this command.

Then, choose the desired components from the checkboxes.
- If you don't know what you're doing just stick with the defaults and go to install
- If you know what you're doing AND you don't wish to change your nvidia driver version, uncheck it (the topmost one)
- You don't necessarily need the examples and documentation and demos, just uncheck them

If it's a success this will be printed:

```
===========
= Summary =
===========

Driver:   Installed
Toolkit:  Installed in /usr/local/cuda-11.6/

Please make sure that
 -   PATH includes /usr/local/cuda-11.6/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-11.6/lib64, or, add /usr/local/cuda-11.6/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-11.6/bin
To uninstall the NVIDIA Driver, run nvidia-uninstall
Logfile is /var/log/cuda-installer.log
```

If it was not a success, make sure you followed the instructions carefully. Make sure you don't have any applications using nvidia drivers including your graphical ui. Open tty and run the commands from there (or from troubleshooting mode if nothing else works). Make sure you have uninstalled all nvidia packages.

If you run into problems with the runfile, check `/var/log/` for the error logs.

---

### Important post-installation steps for CUDA

https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#mandatory-actions

you can check your `PATH` variable like this:

```
echo ${PATH}
```

and you can check `LD_LIBRARY_PATH` similarly:

```
echo ${LD_LIBRARY_PATH}
```

Update these with CUDA paths so the newly installed CUDA will be found when you use it as a dependency in your projects. For example:

```
export PATH=/usr/local/cuda-11.6/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.6/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

Please replace `11.6` with whatever version you have. You can double check that these files exist if you don't remember what you have installed or you get some errors later on:

```
ls /usr/local/cuda-11.6
```

(again, replace `11.6` with whatever version you use).

That's it!

## TensorRT

## cuDNN

I tried a totally new way for this:
- Download the tar from [here](https://developer.nvidia.com/rdp/cudnn-download)
- Follow [this guide](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#installlinux-tar)

and then it's just magically installed. Quite easy. No apt required.


## Other

I have built pytorch from source and enabled CUDA, cuDNN, TensorRT support with the build flags. Thus, I have to rebuild and reinstall it after updating my nvidia drivers and everything else along that.
Similarly I have built opencv from source to enable CUDA support.

---

The end! I hope this helped you. The first few times I installed the Nviida stack were difficult but it gets more easy every time.

I would recommend you to disable any updates to `nvidia-driver-xyz`, `cuda`, `tensorrt`, `cudnn`. In addition, do not download any package from `apt` that might interfere with the aforementioned packages or one that you built from source (such as `opencv` or `pytorch`).

Ubuntu unattended upgrades might mess up your system without asking you. You can disable them or freeze certain packages to prevent this from happening. I would suggest you to read carefully every time you're installing or removing something. `apt` does have these helpful prints that tell which packages are affected.

In ideal world you could just download everything from `apt` and the software would just magically work. Or even better - that you wouldn't have to care about that. Unfortunately life isn't always that easy. Especially if you're developing ML and you have to run neural network training or inference fast on your PC.

But the other side is that you will learn how Ubuntu packages work under the hood, how your system detects your GPU drivers, how software works together and more. So take it as an opportunity to learn - not a scary annoing thing that prevents you from minding your own business.