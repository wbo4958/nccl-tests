cmake_minimum_required(VERSION 3.12)

set (CUDF_JNI_ROOT cudf_jni)
set (THIRD_PARTY /home/bobwang/work.d/nvspark/cudf/thirdparty)

set(CMAKE_CUDA_COMPILER /usr/local/cuda/bin/nvcc)
project("nccl-tests" VERSION 0.7.2 LANGUAGES C CXX CUDA)

add_compile_definitions(__CUDACC__ MPI_SUPPORT=1 NCCL_MAJOR=2 NCCL_MINOR=7)

find_package(CUDA REQUIRED)

set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS}
        -gencode arch=compute_70,code=sm_70 //计算能力的要求
        -rdc=true
        -g
        -G
        --std c++11)

############################### rabit
file(GLOB_RECURSE SRC_SOURCES
        src/*.cc
        src/*.h
        src/*.cu
        )
list(APPEND SOURCE_FILES ${SRC_SOURCES})
include_directories(/home/bobwang/work.d/nvspark/library/ompi/include)

include_directories(/home/bobwang/work.d/nvspark/library/nccl/nccl_2.7.8-1+cuda10.1_x86_64/include)

add_library(cudf SHARED ${SOURCE_FILES})
