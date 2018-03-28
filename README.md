# Pytorch OSX Build

Unfortunately, the Pytorch team does not release binary package for Mac OS with CUDA support. This project provides off-the-shelf binary packages. ``Both Python 2.7 and 3.6 are supported now!``


很不幸，Pytorch团队不发布 Mac OS CUDA版。本项目提供 Mac OS 上编译好、可直接安装的Pytorch CUDA版本。``本项目同时支持Python 2.7 和 3.6 了！``


# Releases


`` Note: The current repo seems to contain bugs about tensor sum due to bugs in the pytorch source code.  Using the old whl files in releases is recommended! I'm waiting for the pytorch team to fix the bugs and re-compile this project.``

``请注意：本次更新似乎包含关于张量加法的 Bug，这是因为pytorch源码中包含了BUG。目前建议使用Release页面中的老版本。在Pytorch团队修复BUG后，我会再次更新本项目。``

| FileName | pytorch | CUDA | CUDNN | Compute Capability | Compilation Time |
|:--:|:--:|:--:|:--:|:--:|:--:|
| torch-0.4.0a0+e3e0c34-cp36-cp36m-macosx\_10\_12_intel.whl | 0.4.0a0+e | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2018-03-27 |
| torch-0.4.0a0+e3e0c34-cp27-cp27m-macosx\_10\_12_intel.whl | 0.4.0a0+e | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2018-03-27 |
| torch-0.4.0a0+ab0a7eb-cp27-cp27m-macosx\_10\_12_intel.whl | 0.4.0a0 | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2017-12-02 |
| torch-0.3.0b0+9622eaa-cp27-cp27m-macosx\_10\_12_intel.whl | 0.3.0b0 | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2017-11-30 |

You can find older releases in the [releases page](https://github.com/TomHeaven/pytorch-osx-build/releases).

你可以在[Releases页面](https://github.com/TomHeaven/pytorch-osx-build/releases)找到以前发布的版本。


# Installation for Python 2.7

First, ensure your CUDA driver and cudnn is installed properly, and copy dependencies in folder `usr_local_lib` to path `/usr/local/lib`.

首先，确保CUDA驱动和cudnn正确安装,并且将文件夹`usr_local_lib`中的依赖项复制到路径`/usr/local/lib`。

```
sudo mkdir /usr/local
sudo mkdir /usr/local/lib
sudo cp usr_local_lib/* /usr/local/lib/
```


Second, uninstall the previous pytorch installtion by

其次，卸载之前版本的pytorch:

```
pip uninstall torch
```

Uncompress and install the binary package from this project:

解压并安装：

```
cat torch* > torch.whl.zip
unzip torch.whl.zip
pip install torch*.whl
```

Install torchvision:

安装torchvision：
```
pip install torchvision*.whl
```

# Installation for Python 3.6

Install Python 3.6 from Homebrew first, and then simply follow the guide for Python 2.7 and replace `pip` command with `pip3` and `python` with `python3`.

首先从Homebrew安装Python 3.6，然后按照Python 2.7的安装步骤执行，注意将`pip`替换为`pip3`，并用`python3`启动`python`。



Enjoy!

开始使用Pytorch吧!


# Source Code

Source code from: [https://github.com/pytorch/pytorch](https://github.com/pytorch/pytorch)