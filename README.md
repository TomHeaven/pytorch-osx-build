# Pytorch OSX Build

Unfortunately, the Pytorch team does not release binary package for Mac OS with CUDA support. This project provides off-the-shelf binary packages. ``Both Python 2.7 and 3.x are supported now!``


很不幸，Pytorch团队不发布 Mac OS CUDA版。本项目提供 Mac OS 上编译好、可直接安装的Pytorch CUDA版本。``本项目同时支持Python 2.7 和 3.x 了！``

# Attenion
+ 【2018.11.30】Compiled with OpenMP support with a guide.
+ 【2018.07.31】Currently, Nvidia provides CUDA 9.2 driver for Mac OS High Sierra (10.13) only. Therefore, do NOT download the CUDA 9.2 versions if you are using Sierra (10.12).


# Releases

| FileName | pytorch | CUDA | CUDNN | Compute Capability | Compilation Time |
|:--:|:--:|:--:|:--:|:--:|:--:|
| torch-1.0.0a0+ff608a9-cp27-cp27m-macosx\_10\_13_intel.whl | 1.0rc1 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-11-30 |
| torch-1.0.0a0+ff608a9-cp37-cp37m-macosx\_10\_13\_x86\_64.whl | 1.0rc1 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-11-30 |
| torch-0.4.1-cp36-cp36m-macosx\_10\_12\_x86\_64.whl | 0.4.1 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-08-01 |
| torch-0.4.1-cp27-cp27m-macosx\_10\_12_intel.whl | 0.4.1 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-08-01 |
| torch-0.4.1-cp36-cp36m-macosx\_10\_12\_x86\_64.whl | 0.4.1 | 9.2 | 7.1 | 3.0,3.5,5.2,6.1 | 2018-07-30 |
| torch-0.4.1-cp27-cp27m-macosx\_10\_12_intel.whl | 0.4.1 | 9.2 | 7.1 | 3.0,3.5,5.2,6.1 | 2018-07-30 |
| torch-0.4.0-cp36-cp36m-macosx\_10\_12\_x86\_64.whl | 0.4.0 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-06-08 |
| torch-0.4.0-cp27-cp27m-macosx\_10\_12_intel.whl | 0.4.0 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-06-08 |
...

You can find releases in  [release page](https://github.com/TomHeaven/pytorch-osx-build/releases).

你可以在[Release页面](https://github.com/TomHeaven/pytorch-osx-build/releases)找到发布版本。


# Installation for Python 2.7

First, ensure your CUDA driver and cudnn is installed properly, and copy dependencies in folder `usr_local_lib` to path `/usr/local/lib`. Also, install OpenMP using Homebrew.

首先，确保CUDA驱动和cudnn正确安装,并且将文件夹`usr_local_lib`中的依赖项复制到路径`/usr/local/lib`。也要通过Homebrew安装OpenMP。

```
sudo mkdir /usr/local
sudo mkdir /usr/local/lib
sudo cp usr_local_lib/* /usr/local/lib/
brew install libomp
brew link --overwrite libomp
```


Second, uninstall the previous pytorch installtion by

其次，卸载之前版本的pytorch:

```
pip uninstall torch
```

Install the wheel package from this project:

安装：

```
pip install torch*.whl
```

Install torchvision:

安装torchvision：
```
pip install -U torchvision
```

# Installation for Python 3.x

Install Python 3.x from Homebrew first, and then simply follow the guide for Python 2.7 and replace `pip` command with `pip3` and `python` with `python3`.

首先从Homebrew安装Python 3.x，然后按照Python 2.7的安装步骤执行，注意将`pip`替换为`pip3`，并用`python3`启动`python`。



Enjoy!

开始使用Pytorch吧!


# Source Code

Source code from: [https://github.com/pytorch/pytorch](https://github.com/pytorch/pytorch)

# Related Links

If you need Tensorflow builds for osx, go to this page: [https://github.com/TomHeaven/tensorflow-osx-build](https://github.com/TomHeaven/tensorflow-osx-build)

如果你需要Tensorflow包，请看这个页面：[https://github.com/TomHeaven/tensorflow-osx-build](https://github.com/TomHeaven/tensorflow-osx-build)