---
layout: post
title:  "To Nvidia Linux Hell and Back: A Survivor's story"
date:   2023-02-20 13:37:00 +0200
tags: nvidia linux gpu ml drivers
author: Eljas Hyyrynen (hyrtsi)
---

I've been to hell and back after installing nvidia drivers, CUDA, cuDNN, TensorRT and building OpenCV from source in Linux.
If you develop applications that use Nvidia GPU acceleration and NN inference in Linux you will probably use [TensorRT](https://github.com/NVIDIA/TensorRT) to optimize the models for CUDA-capable GPUs.

It requires you to do the following:
- Install a single nvidia driver version and stick to it
- Install a compatible CUDA toolkit version
- Install a compatible cuDNN version
- Install a compatible TensorRT version

All these versions must remain more or less the same.
If you wish to upgrade (or if your OS upgrades them without asking you) you might have to install all the rest of the dependencies again.
If you change your TensorRT installation you have to redo your inference engines.

In addition, if you happen to use OpenCV (like many computer vision projects nowadays do) you might have to rebuild it.

After doing this a dozen times it doesn't feel like such a big deal.
But you will waste a couple of hours every time you do so.

## Solution 1: disable unattended upgrades

If your OS installs packages for you, consider disabling that behavior.
It might not be secure unless you remember install the security updates yourself.

Just edit this file

```
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
```

and change the unattended-upgrade to `0`.

## Solution 2: hold all updates related to nvidia driver

You can see all Nvidia-related packages like this:

```
sudo dpkg -l | grep -E nvidia\|tensorrt\|cuda\|cudnn
```

for me the output of that command looks like this:

```
hi  cuda-toolkit-11-8-config-common                             11.8.89-1                                                      all          Common config package for CUDA Toolkit 11.8.
hi  cuda-toolkit-11-config-common                               11.8.89-1                                                      all          Common config package for CUDA Toolkit 11.
hi  cuda-toolkit-config-common                                  12.0.146-1                                                     all          Common config package for CUDA Toolkit.
hi  graphsurgeon-tf                                             8.5.3-1+cuda11.8                                               amd64        GraphSurgeon for TensorRT package
hi  libcudnn8                                                   8.8.0.121-1+cuda12.0                                           amd64        cuDNN runtime libraries
hi  libcudnn8-dev                                               8.8.0.121-1+cuda12.0                                           amd64        cuDNN development libraries and headers
hi  libnvidia-cfg1-510:amd64                                    510.108.03-0ubuntu1                                            amd64        NVIDIA binary OpenGL/GLX configuration library
hi  libnvidia-common-510                                        510.108.03-0ubuntu1                                            all          Shared files used by the NVIDIA libraries
hi  libnvidia-compute-510:amd64                                 510.108.03-0ubuntu1                                            amd64        NVIDIA libcompute package
ii  libnvidia-compute-510:i386                                  510.108.03-0ubuntu1                                            i386         NVIDIA libcompute package
rc  libnvidia-compute-515:amd64                                 515.86.01-0ubuntu1                                             amd64        NVIDIA libcompute package
hi  libnvidia-decode-510:amd64                                  510.108.03-0ubuntu1                                            amd64        NVIDIA Video Decoding runtime libraries
ii  libnvidia-decode-510:i386                                   510.108.03-0ubuntu1                                            i386         NVIDIA Video Decoding runtime libraries
hi  libnvidia-encode-510:amd64                                  510.108.03-0ubuntu1                                            amd64        NVENC Video Encoding runtime library
ii  libnvidia-encode-510:i386                                   510.108.03-0ubuntu1                                            i386         NVENC Video Encoding runtime library
hi  libnvidia-extra-510:amd64                                   510.108.03-0ubuntu1                                            amd64        Extra libraries for the NVIDIA driver
hi  libnvidia-fbc1-510:amd64                                    510.108.03-0ubuntu1                                            amd64        NVIDIA OpenGL-based Framebuffer Capture runtime library
ii  libnvidia-fbc1-510:i386                                     510.108.03-0ubuntu1                                            i386         NVIDIA OpenGL-based Framebuffer Capture runtime library
hi  libnvidia-gl-510:amd64                                      510.108.03-0ubuntu1                                            amd64        NVIDIA OpenGL/GLX/EGL/GLES GLVND libraries and Vulkan ICD
ii  libnvidia-gl-510:i386                                       510.108.03-0ubuntu1                                            i386         NVIDIA OpenGL/GLX/EGL/GLES GLVND libraries and Vulkan ICD
hi  libnvinfer-dev                                              8.5.3-1+cuda11.8                                               amd64        TensorRT development libraries and headers
hi  libnvinfer8                                                 8.5.3-1+cuda11.8                                               amd64        TensorRT runtime libraries
ii  nv-tensorrt-repo-ubuntu2004-cuda11.4-trt8.2.5.1-ga-20220505 1-1                                                            amd64        nv-tensorrt repository configuration files
hi  nvidia-compute-utils-510                                    510.108.03-0ubuntu1                                            amd64        NVIDIA compute utilities
rc  nvidia-compute-utils-515                                    515.86.01-0ubuntu1                                             amd64        NVIDIA compute utilities
hi  nvidia-dkms-510                                             510.108.03-0ubuntu1                                            amd64        NVIDIA DKMS package
rc  nvidia-dkms-515                                             515.86.01-0ubuntu1                                             amd64        NVIDIA DKMS package
hi  nvidia-driver-510                                           510.108.03-0ubuntu1                                            amd64        NVIDIA driver metapackage
hi  nvidia-kernel-common-510                                    510.108.03-0ubuntu1                                            amd64        Shared files used with the kernel module
rc  nvidia-kernel-common-515                                    515.86.01-0ubuntu1                                             amd64        Shared files used with the kernel module
hi  nvidia-kernel-source-510                                    510.108.03-0ubuntu1                                            amd64        NVIDIA kernel source package
ii  nvidia-prime                                                0.8.16~0.20.04.2                                               all          Tools to enable NVIDIA's Prime
ii  nvidia-settings                                             525.85.12-0ubuntu1                                             amd64        Tool for configuring the NVIDIA graphics driver
hi  nvidia-utils-510                                            510.108.03-0ubuntu1                                            amd64        NVIDIA driver support binaries
hi  onnx-graphsurgeon                                           8.5.3-1+cuda11.8                                               amd64        ONNX GraphSurgeon for TensorRT package
ii  screen-resolution-extra                                     0.18build1                                                     all          Extension for the nvidia-settings control panel
ii  xserver-xorg-video-nvidia-510                               510.108.03-0ubuntu1                                            amd64        NVIDIA binary Xorg driver
```

You can hold a single package's updates like this:

```
sudo apt-mark hold nvidia-driver-510
```

For all the ways to hold packages look [here](https://askubuntu.com/questions/18654/how-to-prevent-updating-of-a-specific-package).

I hope after reading this you will never have to fight with broken Nvidia packages due to unattended or mistaken upgrade.

### Final words

If you are careful with your system the packages will almost always work.
If you're working with Docker or similar it may be more easy for you to develop ML since it may be more systematic approach to the craft.
I seldom have problems with my system because I read carefully the output each time when I'm installing or removing a package in `apt`.
I have tested several `nvidia-driver`, CUDA and TensorRT versions and found out that it's easy to find compatible Nvidia libraries even though the support matrix is really confusing.
