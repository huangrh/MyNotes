# https://www.learnpytorch.io/00_pytorch_fundamentals/
- https://pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html  
- https://www.geeksforgeeks.org/python-pytorch-backward-function/   # Backward   







# How to setup RTX 3090 for Pytorch

## 1. Buy an RTX 3090 GPU powered computer.   

## 2. Install cuda toolkit
- Download and install the latest microsoft visual studio community edition (2019 is the current edition).   
- Download and install CUDA Toolkit.  https://developer.nvidia.com/cuda-downloads  
- https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local
## 3. install python and setup environment  
(Link for windows 10 ) 
- Download and install Python if you didn't do so  
- Create an python environment and activate the environment
> python -m venv pyenv  # customize the name ‘pyenv’ as you like. 
> pyenv\Scripts\activate
## 4. Install Pytorch
> pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# use this reference: https://pytorch.org/get-started/locally/

> pip3 install torch==1.10.0+cu113 torchvision==0.11.1+cu113 torchaudio===0.10.0+cu113 -f https://download.pytorch.org/whl/cu113/torch_stable.html
## 5. Verify the installation    
```py
import torch
torch.cuda.is_available()
```

## 5 other  
> pip install fastbook   
- fix graphvis module error when import fastbook
> pip install graphviz pydot  pydotplus
> dot -c # to register from the command line
