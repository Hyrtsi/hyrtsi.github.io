---
layout: post
title:  "Fix Pytorch Error with Yolov6"
date:   2022-07-05 13:37:00 +0200
---

I was testing the new yolov6 repo and I confronted this error when trying to export a model:

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

So I created a new venv with python3.7. Then I downloaded the requiremets:

```
pip install
```



--------


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
(.venv) foo@bar:~/source/YOLOv6$ pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116
(.venv) foo@bar:~/source/YOLOv6$ python3 deploy/ONNX/export_onnx.py --weights yolov6s.pt --img 640 --batch 1
Namespace(batch_size=1, conf_thres=0.25, device='0', end2end=False, half=False, img_size=[640, 640], inplace=False, iou_thres=0.45, max_wh=None, simplify=False, topk_all=100, weights='yolov6s.pt')
Loading checkpoint from yolov6s.pt
/home/eljashyyrynen/source/YOLOv6/.venv/lib/python3.7/site-packages/torch/cuda/__init__.py:146: UserWarning: 
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
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 579, in _apply
    module._apply(fn)
  [Previous line repeated 1 more time]
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 602, in _apply
    param_applied = fn(param)
  File "/home/.../source/YOLOv6/.venv/lib/python3.7/site-packages/torch/nn/modules/module.py", line 763, in <lambda>
    return self._apply(lambda t: t.float() if t.is_floating_point() else t)
RuntimeError: CUDA error: no kernel image is available for execution on the device
CUDA kernel errors might be asynchronously reported at some other API call,so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1.
(.venv) foo@bar:~/source/YOLOv6$ pip3 install torch==1.9.0+cu117 torchvision==0.10.0+cu117 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html
Looking in links: https://download.pytorch.org/whl/torch_stable.html
ERROR: Could not find a version that satisfies the requirement torch==1.9.0+cu117 (from versions: 0.4.1, 0.4.1.post2, 1.0.0, 1.0.1, 1.0.1.post2, 1.1.0, 1.2.0, 1.2.0+cpu, 1.2.0+cu92, 1.3.0, 1.3.0+cpu, 1.3.0+cu100, 1.3.0+cu92, 1.3.1, 1.3.1+cpu, 1.3.1+cu100, 1.3.1+cu92, 1.4.0, 1.4.0+cpu, 1.4.0+cu100, 1.4.0+cu92, 1.5.0, 1.5.0+cpu, 1.5.0+cu101, 1.5.0+cu92, 1.5.1, 1.5.1+cpu, 1.5.1+cu101, 1.5.1+cu92, 1.6.0, 1.6.0+cpu, 1.6.0+cu101, 1.6.0+cu92, 1.7.0, 1.7.0+cpu, 1.7.0+cu101, 1.7.0+cu110, 1.7.0+cu92, 1.7.1, 1.7.1+cpu, 1.7.1+cu101, 1.7.1+cu110, 1.7.1+cu92, 1.7.1+rocm3.7, 1.7.1+rocm3.8, 1.8.0, 1.8.0+cpu, 1.8.0+cu101, 1.8.0+cu111, 1.8.0+rocm3.10, 1.8.0+rocm4.0.1, 1.8.1, 1.8.1+cpu, 1.8.1+cu101, 1.8.1+cu102, 1.8.1+cu111, 1.8.1+rocm3.10, 1.8.1+rocm4.0.1, 1.9.0, 1.9.0+cpu, 1.9.0+cu102, 1.9.0+cu111, 1.9.0+rocm4.0.1, 1.9.0+rocm4.1, 1.9.0+rocm4.2, 1.9.1, 1.9.1+cpu, 1.9.1+cu102, 1.9.1+cu111, 1.9.1+rocm4.0.1, 1.9.1+rocm4.1, 1.9.1+rocm4.2, 1.10.0, 1.10.0+cpu, 1.10.0+cu102, 1.10.0+cu111, 1.10.0+cu113, 1.10.0+rocm4.0.1, 1.10.0+rocm4.1, 1.10.0+rocm4.2, 1.10.1, 1.10.1+cpu, 1.10.1+cu102, 1.10.1+cu111, 1.10.1+cu113, 1.10.1+rocm4.0.1, 1.10.1+rocm4.1, 1.10.1+rocm4.2, 1.10.2, 1.10.2+cpu, 1.10.2+cu102, 1.10.2+cu111, 1.10.2+cu113, 1.10.2+rocm4.0.1, 1.10.2+rocm4.1, 1.10.2+rocm4.2, 1.11.0, 1.11.0+cpu, 1.11.0+cu102, 1.11.0+cu113, 1.11.0+cu115, 1.11.0+rocm4.3.1, 1.11.0+rocm4.5.2, 1.12.0, 1.12.0+cpu, 1.12.0+cu102, 1.12.0+cu113, 1.12.0+cu116, 1.12.0+rocm5.0, 1.12.0+rocm5.1.1)
ERROR: No matching distribution found for torch==1.9.0+cu117
(.venv) foo@bar:~/source/YOLOv6$ pip3 install torch==1.8.0+cu111 torchvision==0.10.0+cu117 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html
Looking in links: https://download.pytorch.org/whl/torch_stable.html
Collecting torch==1.8.0+cu111
  Downloading https://download.pytorch.org/whl/cu111/torch-1.8.0%2Bcu111-cp37-cp37m-linux_x86_64.whl (1982.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.0/2.0 GB 2.5 MB/s eta 0:00:00
ERROR: Could not find a version that satisfies the requirement torchvision==0.10.0+cu117 (from versions: 0.1.6, 0.1.7, 0.1.8, 0.1.9, 0.2.0, 0.2.1, 0.2.2, 0.2.2.post2, 0.2.2.post3, 0.3.0, 0.4.0, 0.4.0+cpu, 0.4.0+cu92, 0.4.1, 0.4.1+cpu, 0.4.1+cu100, 0.4.1+cu92, 0.4.2, 0.4.2+cpu, 0.4.2+cu100, 0.4.2+cu92, 0.5.0, 0.5.0+cpu, 0.5.0+cu100, 0.5.0+cu92, 0.6.0, 0.6.0+cpu, 0.6.0+cu101, 0.6.0+cu92, 0.6.1, 0.6.1+cpu, 0.6.1+cu101, 0.6.1+cu92, 0.7.0, 0.7.0+cpu, 0.7.0+cu101, 0.7.0+cu92, 0.8.0, 0.8.1, 0.8.1+cpu, 0.8.1+cu101, 0.8.1+cu110, 0.8.1+cu92, 0.8.2, 0.8.2+cpu, 0.8.2+cu101, 0.8.2+cu110, 0.8.2+cu92, 0.9.0, 0.9.0+cpu, 0.9.0+cu101, 0.9.0+cu111, 0.9.1, 0.9.1+cpu, 0.9.1+cu101, 0.9.1+cu102, 0.9.1+cu111, 0.10.0, 0.10.0+cpu, 0.10.0+cu102, 0.10.0+cu111, 0.10.0+rocm4.1, 0.10.0+rocm4.2, 0.10.1, 0.10.1+cpu, 0.10.1+cu102, 0.10.1+cu111, 0.10.1+rocm4.1, 0.10.1+rocm4.2, 0.11.0, 0.11.0+cpu, 0.11.0+cu102, 0.11.0+cu111, 0.11.0+cu113, 0.11.0+rocm4.1, 0.11.0+rocm4.2, 0.11.1, 0.11.1+cpu, 0.11.1+cu102, 0.11.1+cu111, 0.11.1+cu113, 0.11.1+rocm4.1, 0.11.1+rocm4.2, 0.11.2, 0.11.2+cpu, 0.11.2+cu102, 0.11.2+cu111, 0.11.2+cu113, 0.11.2+rocm4.1, 0.11.2+rocm4.2, 0.11.3, 0.11.3+cpu, 0.11.3+cu102, 0.11.3+cu111, 0.11.3+cu113, 0.11.3+rocm4.1, 0.11.3+rocm4.2, 0.12.0, 0.12.0+cpu, 0.12.0+cu102, 0.12.0+cu113, 0.12.0+cu115, 0.12.0+rocm4.3.1, 0.12.0+rocm4.5.2, 0.13.0, 0.13.0+cpu, 0.13.0+cu102, 0.13.0+cu113, 0.13.0+cu116, 0.13.0+rocm5.0, 0.13.0+rocm5.1.1)
ERROR: No matching distribution found for torchvision==0.10.0+cu117
(.venv) foo@bar:~/source/YOLOv6$ pip3 install torch==1.8.0+cu111 torchvision==0.10.0+cu111 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html
Looking in links: https://download.pytorch.org/whl/torch_stable.html
Collecting torch==1.8.0+cu111
  Using cached https://download.pytorch.org/whl/cu111/torch-1.8.0%2Bcu111-cp37-cp37m-linux_x86_64.whl (1982.2 MB)
Collecting torchvision==0.10.0+cu111
  Downloading https://download.pytorch.org/whl/cu111/torchvision-0.10.0%2Bcu111-cp37-cp37m-linux_x86_64.whl (23.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 23.2/23.2 MB 7.9 MB/s eta 0:00:00
Collecting torchaudio==0.9.0
  Downloading torchaudio-0.9.0-cp37-cp37m-manylinux1_x86_64.whl (1.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.9/1.9 MB 6.1 MB/s eta 0:00:00
Requirement already satisfied: numpy in ./.venv/lib/python3.7/site-packages (from torch==1.8.0+cu111) (1.21.6)
Requirement already satisfied: typing-extensions in ./.venv/lib/python3.7/site-packages (from torch==1.8.0+cu111) (4.3.0)
Requirement already satisfied: pillow>=5.3.0 in ./.venv/lib/python3.7/site-packages (from torchvision==0.10.0+cu111) (9.2.0)
INFO: pip is looking at multiple versions of <Python from Requires-Python> to determine which version is compatible with other requirements. This could take a while.
INFO: pip is looking at multiple versions of torch to determine which version is compatible with other requirements. This could take a while.
ERROR: Cannot install torch==1.8.0+cu111 and torchvision==0.10.0+cu111 because these package versions have conflicting dependencies.

The conflict is caused by:
    The user requested torch==1.8.0+cu111
    torchvision 0.10.0+cu111 depends on torch==1.9.0

To fix this you could try to:
1. loosen the range of package versions you've specified
2. remove package versions to allow pip attempt to solve the dependency conflict

ERROR: ResolutionImpossible: for help visit https://pip.pypa.io/en/latest/topics/dependency-resolution/#dealing-with-dependency-conflicts
(.venv) foo@bar:~/source/YOLOv6$ pip3 install torch==1.9.0+cu111 torchvision==0.10.0+cu111 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html

(.venv) eljashyyrynen@fidp21aloy:~/source/YOLOv6$ python3 deploy/ONNX/export_onnx.py --weights yolov6s.pt --img 640 --batch 1
Namespace(batch_size=1, conf_thres=0.25, device='0', end2end=False, half=False, img_size=[640, 640], inplace=False, iou_thres=0.45, max_wh=None, simplify=False, topk_all=100, weights='yolov6s.pt')
Loading checkpoint from yolov6s.pt

Fusing model...

Starting to export ONNX...
/home/eljashyyrynen/source/YOLOv6/yolov6/models/effidehead.py:76: TracerWarning: Converting a tensor to a Python boolean might cause the trace to be incorrect. We can't record the data flow of Python values, so this value will be treated as a constant in the future. This means that the trace might not generalize to other inputs!
  if self.grid[i].shape[2:4] != y.shape[2:4]:
ONNX export success, saved as yolov6s.onnx

Export complete (3.32s)

---

eljashyyrynen@fidp21aloy:~/source/YOLOv6$ python3
Python 3.8.10 (default, Mar 15 2022, 12:22:08) 
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.cuda.get_arch_list()
['sm_37', 'sm_50', 'sm_60', 'sm_70']
>>> print(torch.__version__)
1.11.0+cu102
>>> 










