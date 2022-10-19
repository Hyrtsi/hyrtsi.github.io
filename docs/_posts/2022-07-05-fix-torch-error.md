---
layout: post
title:  "Fix Pytorch Error with Yolov6"
date:   2022-07-05 13:37:00 +0200
tags: pytorch yolov6 ml
---

I was testing the [new yolov6 repo](https://github.com/meituan/YOLOv6) and I confronted this error when trying to export a model:

```
$ python3 deploy/ONNX/export_onnx.py --weights yolov6s.pt --img 640 --batch 1
Namespace(batch_size=1, conf_thres=0.25, device='0', end2end=False, half=False, img_size=[640, 640], inplace=False, iou_thres=0.45, max_wh=None, simplify=False, topk_all=100, weights='yolov6s.pt')
Loading checkpoint from yolov6s.pt
<...>YOLOv6/.venv/lib/python3.7/site-packages/torch/cuda/__init__.py:146: UserWarning: 
NVIDIA GeForce RTX 3080 Laptop GPU with CUDA capability sm_86 is not compatible with the current PyTorch installation.
The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_70.
If you want to use the NVIDIA GeForce RTX 3080 Laptop GPU GPU with PyTorch, please check the instructions at https://pytorch.org/get-started/locally/

  warnings.warn(incompatible_device_warn.format(device_name, capability, " ".join(arch_list), device_name))
Traceback (most recent call last):
  File "deploy/ONNX/export_onnx.py", line 47, in <module>
    model = load_checkpoint(args.weights, map_location=device, inplace=True, fuse=True)  # load FP32 model
  File "<...>YOLOv6/yolov6/utils/checkpoint.py", line 26, in load_checkpoint
    model = ckpt['ema' if ckpt.get('ema') else 'model'].float()
  File "<...>YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 763, in float
    return self._apply(lambda t: t.float() if t.is_floating_point() else t)
  File "<...>YOLOv6/yolov6/models/yolo.py", line 41, in _apply
    self = super()._apply(fn)
  File "<...>YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 579, in _apply
    module._apply(fn)
  File "<...>YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 579, in _apply
    module._apply(fn)
  File "<...>YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 579, in _apply
    module._apply(fn)
  [Previous line repeated 1 more time]
  File "<...>YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 602, in _apply
    param_applied = fn(param)
  File "<...>YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 763, in <lambda>
    return self._apply(lambda t: t.float() if t.is_floating_point() else t)
RuntimeError: CUDA error: no kernel image is available for execution on the device
CUDA kernel errors might be asynchronously reported at some other API call,so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1.
```

That's a bummer. I checked my `torch` version and supported architectures like this:

```
$ python3
Python 3.8.10 (default, Mar 15 2022, 12:22:08) 
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.cuda.get_arch_list()
['sm_37', 'sm_50', 'sm_60', 'sm_70']
>>> print(torch.__version__)
1.11.0+cu102
```

So I created a new [venv](https://docs.python.org/3/library/venv.html) with [python3.7](https://www.python.org/downloads/). Then I downloaded the requirements:

```
(.venv) foo@bar:~/source/YOLOv6$ pip install -r requirements.txt
(.venv) foo@bar:~/source/YOLOv6$ pip install --upgrade pip
(.venv) foo@bar:~/source/YOLOv6$ python3 deploy/ONNX/export_onnx.py --weights yolov6s.pt --img 640 --batch 1
Namespace(batch_size=1, conf_thres=0.25, device='0', end2end=False, half=False, img_size=[640, 640], inplace=False, iou_thres=0.45, max_wh=None, simplify=False, topk_all=100, weights='yolov6s.pt')
Loading checkpoint from yolov6s.pt
/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/cuda/__init__.py:146: UserWarning: 
NVIDIA GeForce RTX 3080 Laptop GPU with CUDA capability sm_86 is not compatible with the current PyTorch installation.
The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_70.
If you want to use the NVIDIA GeForce RTX 3080 Laptop GPU GPU with PyTorch, please check the instructions at https://pytorch.org/get-started/locally/

  warnings.warn(incompatible_device_warn.format(device_name, capability, " ".join(arch_list), device_name))
Traceback (most recent call last):
  File "deploy/ONNX/export_onnx.py", line 47, in <module>
    model = load_checkpoint(args.weights, map_location=device, inplace=True, fuse=True)  # load FP32 model
  File "/home/.../source/YOLOv6/yolov6/utils/checkpoint.py", line 26, in load_checkpoint
    model = ckpt['ema' if ckpt.get('ema') else 'model'].float()
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 763, in float
    return self._apply(lambda t: t.float() if t.is_floating_point() else t)
  File "/home/.../source/YOLOv6/yolov6/models/yolo.py", line 41, in _apply
    self = super()._apply(fn)
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 579, in _apply
    module._apply(fn)
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 579, in _apply
    module._apply(fn)
  File "/home/../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 579, in _apply
    module._apply(fn)
  [Previous line repeated 1 more time]
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 602, in _apply
    param_applied = fn(param)
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 763, in <lambda>
    return self._apply(lambda t: t.float() if t.is_floating_point() else t)
RuntimeError: CUDA error: no kernel image is available for execution on the device
CUDA kernel errors might be asynchronously reported at some other API call,so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1.
```

so I got basically the same error again. Not good. I found out, however, how to fix this. Just do this after installing the dependencies:

```
pip install torch==1.12.0+cu116 torchvision==0.13.0+cu116 -f https://download.pytorch.org/whl/torch_stable.html
```

After that the export will work just fine:

```
(.venv) foo@bar:~/source/YOLOv6$ python3 deploy/ONNX/export_onnx.py --weights yolov6s.pt --img 640 --batch 1
Namespace(batch_size=1, conf_thres=0.25, device='0', end2end=False, half=False, img_size=[640, 640], inplace=False, iou_thres=0.45, max_wh=None, simplify=False, topk_all=100, weights='yolov6s.pt')
Loading checkpoint from yolov6s.pt

Fusing model...

Starting to export ONNX...
/home/xyz/source/YOLOv6/yolov6/models/effidehead.py:76: TracerWarning: Converting a tensor to a Python boolean might cause the trace to be incorrect. We can't record the data flow of Python values, so this value will be treated as a constant in the future. This means that the trace might not generalize to other inputs!
  if self.grid[i].shape[2:4] != y.shape[2:4]:
ONNX export success, saved as yolov6s.onnx

Export complete (3.32s)
```

I found the answer [here](https://github.com/meituan/YOLOv6/issues/350)







