# Pytorch OSX Build

Unfortunately, the Pytorch team does not release binary package for Mac OS with CUDA support. This project provides off-the-shelf binary packages.

很不幸，Pytorch团队不发布 Mac OS CUDA版。本项目提供 Mac OS 上编译好、可直接安装的的Pytorch CUDA版本。


# Releases

| FileName | pytorch | CUDA | CUDNN | Compute Compatibility | Compilation Time |
|:--:|:--:|:--:|:--:|:--:|:--:|
| torch-0.3.0b0+9622eaa-cp27-cp27m-macosx\_10\_12_intel.whl | 0.3.0b0 | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2017-11-30 |


# Installation

First, ensure your CUDA driver and cudnn is installed properly.

首先，确保CUDA驱动和cudnn正确安装。

Second, uninstall the previous tensorflow installtion by

其次，卸载之前版本的Tensorflow:

```
pip uninstall torch
```

At last, uncompress and install the binary package from this project:

解压并安装：

```
cat torch* > torch.whl.zip
unzip torch.whl.zip
pip install torch*.whl
```

安装torchvision
```
pip install torchvision*.whl
```

Enjoy!

开始使用Pytorch吧!


# Source Code

Source code from: [https://github.com/pytorch/pytorch](https://github.com/pytorch/pytorch)