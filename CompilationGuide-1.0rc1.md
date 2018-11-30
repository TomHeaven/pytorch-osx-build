# Compilation Guide of Pytorch 1.0rc1 on Mac OS X

If you only want to use pytorch on Mac with CUDA support, first go to the page: https://github.com/TomHeaven/tensorflow-osx-build/releases and try to find proper wheel pakcages. 

However, as their are mulitple versions of python (2.7, 3.6, 3.7) and CUDA (8.0, 9.0, 9.2, 10.0). It's more and more time consuming to support all combinations of them. Sometimes, you may need to compile a wheel package yourself.

This guide will help you compile a wheel package of pytorch-1.0rc1 on Mac OS X with CUDA support. 

## Dependencies
+ XCode 8.0. 
+ CUDA 9.0 and CuDNN7.0 (CUDA 9.2 and CuDNN7.1 should also work, but not tested).
+ OpenMP. Install using Homebrew:

```shell
brew install libomp
brew link --overwrite libomp
```


## Clone Pytorch Repo
```
git clone https://github.com/pytorch/pytorch
git checkout v1.0rc1
git submodule update --init
```

## Update Dependency.cmake


Modify cmake/Dependency.cmake:448 as

```CMake
# ---[ OpenMP
#if(USE_OPENMP)
  #find_package(OpenMP)
    # see https://github.com/Kitware/CMake/blob/master/Modules/FindOpenMP.cmake
  #(OPENMP_FOUND TRUE)
  set(OpenMP_C_FLAGS " -Xpreprocessor -fopenmp -lomp -lgomp ")
  set(OpenMP_CXX_FLAGS " -Xpreprocessor -fopenmp -lomp -lgomp ")
#endif() 
  ####
  #if(OPENMP_FOUND)
    message(STATUS "Adding " ${OpenMP_CXX_FLAGS})
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
  #else()
  # message(WARNING "Not compiling with OpenMP. Suppress this warning with -DUSE_OPENMP=OFF")
  # caffe2_update_option(USE_OPENMP OFF)
  #endif()
#endif()
```

## Compile

Compile using

``` shell
# specify version
export PYTORCH_BUILD_VERSION=1.0
export PYTORCH_BUILD_NUMBER=1
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

