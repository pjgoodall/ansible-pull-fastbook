# ansible-pull-fastbook

This is a work-in-progress. Comments are welcome, but it is now **very pre-alpha**.

The artifacts here will install a fastbook learning environment into 
an Ubuntu lxc container or workstation/server


## Development progress/process

### Ubuntu/lxc

Expecting Ubuntu LTS >= 18.04

Current development is within an lxc container running ubuntu:20.04. I will provide a profile and documentation so you can launch a container with ansible installed

## Host Install

### Host requirements

Operating System - Ubuntu LTS >= 18.04 (including 20.04.x)

You must have a supported Graphics card. The card needs to be supported within
- Nvidia Drivers
- CUDA
- The version of PyTorch used in the fastbook software.

[Verify you have a cuda-capable system](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#verify-you-have-cuda-enabled-system)

My system:

``` bash
% lspci | grep -i nvidia
01:00.0 VGA compatible controller: NVIDIA Corporation GP107 [GeForce GTX 1050 Ti] (rev a1)
01:00.1 Audio device: NVIDIA Corporation GP107GL High Definition Audio Controller (rev a1)
(lxc-x11) 
```
Look this up at [https://developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus).

My specific card `GeForce GTX 1050 Ti` is not explicitly in the list, but it seems that Nvidia is not maintaining their compatibility list properly `:-(` 

It runs fine:

```
% nvidia-smi
Fri Nov 20 10:21:40 2020       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 455.38       Driver Version: 455.38       CUDA Version: 11.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce GTX 105...  Off  | 00000000:01:00.0  On |                  N/A |
| 43%   41C    P0    N/A /  75W |   1065MiB /  4032MiB |      1%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
[...]
```


The [`environment.yml`](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file) file in the [fastbook repository](https://github.com/fastai/fastbook) specifies the pytorch version as `pytorch>=1.6` The current stable version of pytorch, and its compatibility is displayed nicely on [pytorch.org](https://pytorch.org/)

Working backwards we need to determine if you have an Nvidia video card that will run a version a version of CUDA >= 10.1 according to [Install Pytorch on pytorch.org](https://pytorch.org/)


### Install Nvidia Driver
I found the recommended driver from the default repository worked well. It appears that CUDA and Nvidia has become mainstream on the Ubuntu LTS platforms.

I recommend the autoinstall from the command-line 

```
sudo ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001C82sv00001462sd00008C96bc03sc00i00
vendor   : NVIDIA Corporation
model    : GP107 [GeForce GTX 1050 Ti]
driver   : nvidia-driver-418-server - distro non-free
driver   : nvidia-driver-440-server - distro non-free
driver   : nvidia-driver-455 - distro non-free recommended
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-390 - distro non-free
driver   : nvidia-driver-450 - distro non-free
driver   : xserver-xorg-video-nouveau - distro free builtin

```


## General References

1. https://datacrunch.io/docs/setting-up-fastai/ - I will feed-back my work to DataCrunch.io
2. [ansible-pull](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html#ansible-pull)
3. [github: fastai/fastbook](https://github.com/fastai/fastbook)

A lot more on the way...