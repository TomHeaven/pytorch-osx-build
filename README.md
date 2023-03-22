# Pytorch OSX Build

Unfortunately, the Pytorch team does not release binary package for Mac OS with CUDA support. This project provides off-the-shelf binary packages.

很不幸，Pytorch团队不发布 Mac OS CUDA版。本项目提供 Mac OS 上编译好、可直接安装的Pytorch CUDA版本。

# Performance Warning
【2020.02.18】I benchmarked Pytorch 1.3.1 with CUDA 10.1 and CUDNN 7.6.5 on Mac OS X 10.13.6 and Ubuntu 16.04, performance on Mac OS is around 2/3 of that on Ubuntu. In addition, it is more likey to encounter "CUDA OUT OF MEMORY" error on Mac OS since the operating system takes a large amount of GPU memory for display. Be aware of this performance difference and if you have a lot of data to process, you would better turn to Ubuntu!

The following table lists training time of MNIST image classification demo.

| Settings  | Single GPU | Dual GPUs |
|-- | -- | -- |
| Ubuntu 1080Ti	| 99.76 s | 50.99 s |
| MacOS 1080Ti	| 156 s   | 82 s    |


# Compile Yourself
If you find the releases cannot meet your requirements, you can compile from source youreself.

+ Guides are avaiable: 
 - [1.0.1](https://github.com/TomHeaven/pytorch-osx-build/blob/master/BuildInstractions-1.0.1.md).
 - [1.0rc1](https://github.com/TomHeaven/pytorch-osx-build/blob/master/BuildInstractions-1.0rc1.md).
+ Source pathces are availabe at `source_pathes` folder of the master branch.



# Releases


You can find releases in  [release page](https://github.com/TomHeaven/pytorch-osx-build/releases).

你可以在[Release页面](https://github.com/TomHeaven/pytorch-osx-build/releases)找到发布版本。


# Installation for Python 2.7 (Outedated)

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

# Installation for Python 3

Install Python 3.x from Homebrew first, and then simply follow the guide for Python 2.7 and replace `pip` command with `pip3` and `python` with `python3`.

首先从Homebrew安装Python 3.x，然后按照Python 2.7的安装步骤执行，注意将`pip`替换为`pip3`，并用`python3`启动`python`。



Enjoy!

开始使用Pytorch吧!


# Source Code

Source code from: [https://github.com/pytorch/pytorch](https://github.com/pytorch/pytorch)

# Related Links

If you need Tensorflow builds for osx, go to this page: [https://github.com/TomHeaven/tensorflow-osx-build](https://github.com/TomHeaven/tensorflow-osx-build)

If you need MxNet builds for osx, go to this page: [https://github.com/TomHeaven/mxnet_osx_build](https://github.com/TomHeaven/mxnet_osx_build)



如果你需要Tensorflow包，请看这个页面：[https://github.com/TomHeaven/tensorflow-osx-build](https://github.com/TomHeaven/tensorflow-osx-build)

如果你需要MxNet包，请看这个页面：[https://github.com/TomHeaven/mxnet_osx_build](https://github.com/TomHeaven/mxnet_osx_build)