project(gflip3d)
cmake_minimum_required(VERSION 2.8)
find_package(CUDA QUIET REQUIRED)
#set(CMAKE_BUILD_TYPE Release)

if (CMAKE_BUILD_TYPE STREQUAL Debug)
LIST(APPEND CUDA_NVCC_FLAGS
  -gencode arch=compute_75,code=compute_75
  -gencode arch=compute_75,code=sm_75
  -lineinfo
  -g
 )
else()
LIST(APPEND CUDA_NVCC_FLAGS -O3
  -gencode arch=compute_75,code=compute_75
  -gencode arch=compute_75,code=sm_75
  -lineinfo
 )
endif()

if(NOT MSVC)
  set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS}  -O3 -ffast-math ")
  set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS}  -ffast-math  -g")
endif()

include_directories(
    GDelFlipping/Main
    )
cuda_add_executable(
    ${PROJECT_NAME}
    GDelFlipping/src/DelaunayChecker.cpp
    GDelFlipping/src/Demo.cpp
    GDelFlipping/src/InputCreator.cpp
    GDelFlipping/src/RandGen.cpp
    GDelFlipping/src/gDel3D/GpuDelaunay.cu
    GDelFlipping/src/gDel3D/CPU/predicates.cpp
    GDelFlipping/src/gDel3D/CPU/PredWrapper.cpp
    GDelFlipping/src/gDel3D/CPU/Splaying.cpp
    GDelFlipping/src/gDel3D/CPU/Star.cpp
    GDelFlipping/src/gDel3D/GPU/KerDivision.cu
    GDelFlipping/src/gDel3D/GPU/KerPredicates.cu
    GDelFlipping/src/gDel3D/GPU/ThrustWrapper.cu
    )
