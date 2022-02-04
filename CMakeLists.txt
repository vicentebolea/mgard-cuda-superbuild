project(mgard-cuda-superbuild)
cmake_minimum_required(VERSION 3.12)

include(ExternalProject)

set(install_path ${CMAKE_BINARY_DIR}/install)

ExternalProject_add(nvcomp
  GIT_REPOSITORY https://github.com/NVIDIA/nvcomp.git
  GIT_TAG main
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${install_path}
  )

ExternalProject_add(zstd
  GIT_REPOSITORY https://github.com/facebook/zstd.git
  GIT_TAG v1.5.0
  SOURCE_SUBDIR build/cmake
  CMAKE_ARGS -DZSTD_MULTITHREAD_SUPPORT=ON -DCMAKE_INSTALL_PREFIX=${install_path}
  )

ExternalProject_add(mgard
  DEPENDS zstd nvcomp
  GIT_REPOSITORY https://github.com/JieyangChen7/MGARD.git
  GIT_TAG mgard-x
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX=${install_path}
    -DZSTD_MULTITHREAD_SUPPORT=ON
    -DMGARD_ENABLE_SERIAL=ON
    -DMGARD_ENABLE_CUDA=ON
    -DMGARD_ENABLE_CUDA_FMA=ON
    -DMGARD_ENABLE_CUDA_OPTIMIZE_VOLTA=ON
    -DMGARD_ENABLE_CUDA_OPTIMIZE_TURING=OFF
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_CUDA_ARCHITECTURES=61
  )
