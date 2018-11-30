# Build Instrctions of Pytorch 1.0rc1 on Mac OS X 10.13


By TomHeaven @ 2018.11.30

---

If you only want to use pytorch on Mac with CUDA support, first go to release page: [https://github.com/TomHeaven/tensorflow-osx-build/releases](https://github.com/TomHeaven/tensorflow-osx-build/releases) and try to find proper wheel pakcages. 

However, as their are mulitple versions of python (2.7, 3.6, 3.7) and CUDA (8.0, 9.0, 9.2, 10.0). It's more and more time consuming to support all combinations of them. If your combination of python and CUDA is not listed in the release page, you may need to compile a wheel package yourself.

This guide will help you compile a wheel package of pytorch-1.0rc1 on Mac OS X 10.13 with CUDA support. 

Note compilation on or for 10.12 will encounter a strange error due to clang does not recognize -fopenmp at 98%. I have no idea how to fix it.

## Dependencies

Mandatory dependencies:

+ Mac OS 10.13.
+ XCode 8.0. (XCode 9.0 requires CUDA 9.2 or above.)
+ CUDA 9.0 and CuDNN7.0 (CUDA 9.2 and CuDNN7.1 should also work, but not tested).
+ OpenMP for multi-thread computation. Install using Homebrew:

```shell
brew install libomp
brew link --overwrite libomp
```

+ Openblas for CPU accelerated matrix computation and so on.

```
brew install openblas
brew link --overwrite openblas
```
You can also use MKL from Intel to replace Openblas (Not tested).

Note at the time of writing this guide, Tensorflow can be compiled but does not work properly with CUDA 9.2. If you also need Tensorflow, use CUDA 9.0.

## Clone Pytorch Repo
Install git and clone pytorch repo by

```
brew install git
git clone https://github.com/pytorch/pytorch
cd pytorch
git checkout v1.0rc1
git submodule update --init
```

## Update Dependency.cmake


The default OpenMP detection will fail. So let's just manually set related buid flags and skip the detection by modify `cmake/Dependency.cmake:448` as

```CMake
# ---[ OpenMP
#if(USE_OPENMP)
  #find_package(OpenMP)
  
  set(OpenMP_C_FLAGS "-Xpreprocessor -fopenmp -I/usr/local/include -L/usr/local/lib -lomp -lgomp")
  set(OpenMP_CXX_FLAGS "-Xpreprocessor -fopenmp -I/usr/local/include -L/usr/local/lib -lomp -lgomp")
  set(OpenMP_EXE_LINKER_FLAGS "-Xpreprocessor -fopenmp -L/usr/local/lib -lomp -lgomp")
  caffe2_update_option(USE_OPENMP ON)

  #if(OPENMP_FOUND)
    message(STATUS "Adding " ${OpenMP_CXX_FLAGS})
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
  #else()
  #  message(WARNING "Not compiling with OpenMP. Suppress this warning with -DUSE_OPENMP=OFF")
   # caffe2_update_option(USE_OPENMP OFF)
  #endif()
#endif()
```



## Compilation

Compile using

```shell
# set CUDA_ARCH
export TORCH_CUDA_ARCH_LIST="3.0;3.5;5.2;6.1"
# compile for python2
pip install pyyaml
python setup.py bdist_wheel
# compile for python3
pip3 install pyyaml
python3 setup.py bdist_wheel
```

The wheel packages will be avaiable at `dist` folder if everything goes smoothly. 

## Installation
Just cd to `dist` and install wheel packages using `pip`.


```shell
cd dist
pip install pytorch-xxx-cp27-xxx.wheel
pip3 install pytorch-xxx-cp3x-xxx.wheel
```

