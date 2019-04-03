# Build Instrctions of Pytorch 1.0.1 on Mac OS X 10.13


By TomHeaven @ 2019.4.3

---

If you only want to use pytorch on Mac with CUDA support, first go to release page: [https://github.com/TomHeaven/pytorch-osx-build/releases](https://github.com/TomHeaven/pytorch-osx-build/releases) and try to find proper wheel pakcages. 

However, as their are mulitple versions of python (2.7, 3.6, 3.7) and CUDA (8.0, 9.0, 9.2, 10.0). It's more and more time consuming to support all combinations of them. If your combination of python and CUDA is not listed in the release page, you may need to compile a wheel package yourself.

This guide will help you compile a wheel package of pytorch-1.0.1 on Mac OS X 10.13 with CUDA support. 


## Dependencies

Dependencies:

+ Mac OS 10.13.x.
+ XCode 9.4.1
+ CUDA 10.0 and CuDNN7.4 
+ Optinally, Openblas for CPU accelerated matrix computation and so on. In previous releases (about 0.3.0 or 0.4.0), Pytorch cannot correctly detect vecLib on Mac OS. So we need Openblas as an alternative. However, this problem seems to be solved now and we no longer have to install openblas.

```
brew install openblas
brew link --overwrite openblas
```

+ Optinally, OpenMP for multi-thread computation. Install using Homebrew:

```shell
brew install libomp
brew link --overwrite libomp
```
+ 【Most of you don't need this.】Optionally, OpenMPI for distributed computation. The default supported backend on Mac OS is `tcp`. With OpenMPI, you can use `mpi` as backend in `torch.distributed.deprecated` (The newly constructed `torch.distributed` does not support Mac OS yet.) and enjoy distributed training with multiple machines and GPUs. 

```
brew install openmpi
brew link --overwrite openmpi
```

Distributed computation sounds great but requires extra efforts in coding. See https://pytorch.org/tutorials/intermediate/dist_tuto.html.

I'm also eagerly waiting for NCCL, a better distributed backend, for Mac OS and opened an issue https://github.com/NVIDIA/nccl/issues/201 there to make developers in Nvidia aware of the request.


## Clone Pytorch Repo
Install git and clone pytorch repo by

```
brew install git
git clone https://github.com/pytorch/pytorch
cd pytorch
git checkout v1.0.1
git submodule update --init
```

## Update Dependency.cmake

`If you do not need OpenMP or MPI for distributed computation, just skip this section and start compiling.`


The default OpenMP detection will fail. If you need OpenMP, manually set related build flags by 
+ modifying `cmake/Dependency.cmake:448` as


```CMake
# ---[ OpenMP
#if(USE_OPENMP)
  #find_package(OpenMP)
  
  set(OpenMP_C_FLAGS "-Xpreprocessor -fopenmp -I/usr/local/include -L/usr/local/lib -lomp -lgomp")
  set(OpenMP_CXX_FLAGS "-Xpreprocessor -fopenmp")
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
to make caffe2 use OpenMP.

+ modify `third_party/ideep/mkl-dnn/cmake/OpenMP.cmake:93` as

```
#set(OpenMP_CXX_FLAGS "-fopenmp")
#set(OpenMP_C_FLAGS "-fopenmp")
set(OpenMP_C_FLAGS "-Xpreprocessor -fopenmp -I/usr/local/include -L/usr/local/lib -lomp -lgomp")
set(OpenMP_CXX_FLAGS "-Xpreprocessor -fopenmp -I/usr/local/include -L/usr/local/lib -lomp -lgomp")
```
to avoid a `-fopenmp` related link error at about 90%.

+ modify `torch/CMakeListst.txt:330` as

```
find_package(OpenMP)
if(OPENMP_FOUND)
  if (VERBOSE)
    message(STATUS "Compiling with OpenMP")
  endif()
  target_compile_options(torch INTERFACE -Xpreprocessor -fopenmp)
  target_link_libraries(torch -Xpreprocessor -fopenmp)
endif()
```
to avoid a `-fopenmp` related link error at about 95%.



## Compilation

Compile using

```shell
# version
export PYTORCH_BUILD_VERSION=1.0.1
export PYTORCH_BUILD_NUMBER=1
# set CUDA_ARCH
export TORCH_CUDA_ARCH_LIST="3.5;3.5;5.2;7.0"
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

