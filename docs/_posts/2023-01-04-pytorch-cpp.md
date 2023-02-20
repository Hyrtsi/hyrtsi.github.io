---
layout: post
title:  "Pytorch and c++: Building Libtorch from source"
date:   2023-02-15 13:37:00 +0200
tags: pytorch ml cpp
author: Eljas Hyyrynen (hyrtsi)
---

Building Pytorch ("Libtorch") from scratch for c++.
Who even does that?
Well, we tried doing it.

Apparently, it's possible even though you may run into trouble.

# The million dollar question: why even bother?

If the "standard" features don't make you happy then it's best to build Pytorch from source.
You may get more control if you do so, however.
Note that for GPU-accelerated Pytorch experience you don't have to build from source.

# The easy way: do not build Pytorch at all.

Are you using only Python for developing ML?
Are you using c++ and CUDA 11.6 or CUDA 11.7? Or not interested in GPU-acceleration at all?
It's your lucky day. 
Just head to [this](https://pytorch.org/get-started/locally/) link and use the official installation mediums to get your copy of Pytorch.

# The hard way: see the world burn

I tried running just this after cloning Pytorch:

```
cmake -S . -B build && cmake --build build -j
```

There are two bad sides:
1. It takes a long long time
2. It does not work

I would suggest you to try the following instead:

```
python3 pytorch/tools/build_libtorch.py --rerun-cmake
```

You need to find out how to set the environment variables if you want to configure your build somehow.
I tested both this installation method and using a pre-built binary.
I was able to train and test my models using the GPU which was enough for me.

I have left [an issue](https://github.com/pytorch/pytorch/issues/91697) to Pytorch of my build issues.
Please consider joining that conversation if you insist on building without the python script mentioned above and you run into similar issues.

Pytorch is currently my favorite ML tool.
Even though most ML happens in Python it's good to know that there is a good tool for c++ ML.

# Sources

- https://michhar.github.io/how-i-built-pytorch-gpu/
- https://github.com/pytorch/pytorch/blob/master/docs/libtorch.rst
- https://gist.github.com/lasagnaphil/3e0099816837318e8e8bcab7edcfd5d9
- https://stackoverflow.com/questions/74944881/building-libtorch-from-source-gives-unexpected-contents
- https://pytorch.org
- https://github.com/pytorch/pytorch
