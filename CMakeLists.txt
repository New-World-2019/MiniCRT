# 设置 cmake 最小版本
cmake_minimum_required(VERSION 3.10)

# 指定项目名和语言 project命令：https://blog.csdn.net/mataojie/article/details/121228251
project(MiniCRT LANGUAGES C CXX)
# MiniCRT_SOURCE_DIR 目录绝对路径 /home/search/OpenSource/MiniCRT
if(${MiniCRT_SOURCE_DIR} STREQUAL ${MiniCRT_BINARY_DIR})
    message(FATAL_ERROR "In-source builds not allowed. Please make a new directory (called a build directory) and run CMake from there.")
endif()

# Set C gnu11
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)  # 默认存放静态库的位置  CMAKE_BINARY_DIR : source/build, 这里是 /home/search/OpenSource/MiniCRT/build
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)  # 默认存放动态库的位置
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)  # 存放可执行文件目录

set(MiniCRT_SOURCE_CODE_DIR ${MiniCRT_SOURCE_DIR}/src)

# 添加一个子目录并构建该子目录
add_subdirectory(${MiniCRT_SOURCE_CODE_DIR})
