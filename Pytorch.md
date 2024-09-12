# Pytorch forward
- https://stackoverflow.com/questions/55338756/why-there-are-different-output-between-model-forwardinput-and-modelinput
```
If you dig into the code of nn.Module class you will see __call__ ultimately calls forward but internally handles the forward or backward hooks and manages some states that pytorch allows. 
```

# Hugging Face
- [YouTube : What is Hugging Face? - Machine Learning Hub Explained](https://www.youtube.com/watch?v=1AUjKfpRZVo)



# https://www.learnpytorch.io/00_pytorch_fundamentals/
- https://pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html  
- https://www.geeksforgeeks.org/python-pytorch-backward-function/   # Backward   
- https://pytorch.org/vision/stable/transforms.html
- https://stackoverflow.com/questions/64629702/pytorch-transform-totensor-changes-image  
```
torchvision.transforms toTensor:
Converts a PIL Image or numpy.ndarray (H x W x C) in the range [0, 255] to a torch.FloatTensor of shape (C x H x W) in the range [0.0, 1.0]
```





# How to setup RTX 3090 for Pytorch

## 1. Buy an RTX 3090 GPU powered computer.   

## 2. Install cuda toolkit
- Download and install the latest microsoft visual studio community edition (2019 is the current edition).   
- https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local
> nvcc --version
> 
## 3. install python and setup environment  
(Link for windows 10 ) 
- Download and install Python if you didn't do so  
- Create an python environment and activate the environment
> python -m venv pyenv  # customize the name ‘pyenv’ as you like. 
> pyenv\Scripts\activate
> 
## 4. Install Pytorch
> pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118   
> pip install torchtext --index-url https://download.pytorch.org/whl/cu118


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
>

## DILL_AVAILABLE not available  
- https://github.com/pytorch/pytorch/pull/122616  
```  
import torch  
torch.utils.data.datapipes.utils.common.DILL_AVAILABLE = torch.utils._import_utils.dill_available()  
import torchdata  
```
