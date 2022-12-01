# 由于 x64 变长参数难以实现，需要借助 builtin 函数
message(STATUS "C compiler: ${CMAKE_C_COMPILER_ID}, version: ${CMAKE_C_COMPILER_VERSION}")
message(STATUS "C++ compiler: ${CMAKE_CXX_COMPILER_ID}, version: ${CMAKE_CXX_COMPILER_VERSION}")

if (CMAKE_C_COMPILER_ID STREQUAL "MSVC")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /Wall")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CMAKE_C_FLAGS} /GR-")
    add_link_options(/ENTRY:mini_crt_entry /NODEFAULTLIB)

else ()
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m64") # CMAKE_C_FLAGS 设置 C 编译选项
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O0 -Wall -Wextra -g -ggdb")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdlib -fno-builtin")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -ffreestanding -fno-exceptions")
    if (CMAKE_C_COMPILER_ID STREQUAL "AppleClang")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CMAKE_C_FLAGS} -fno-rtti")
        add_link_options(-e _mini_crt_entry -lSystem -nostartfiles)
    elseif (CMAKE_C_COMPILER_ID STREQUAL "GNU")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CMAKE_C_FLAGS} -fno-rtti -nostdinc++")
        add_link_options(-e mini_crt_entry -nostartfiles)
    endif ()
endif ()

message(STATUS "C FLAGS: ${CMAKE_C_FLAGS}")
message(STATUS "CXX FLAGS: ${CMAKE_CXX_FLAGS}")

# 将指定目录添加到编译器的头文件搜索路径之下，指定的目录被解释成当前源码路径的相对路径。
include_directories(${MiniCRT_SOURCE_CODE_DIR}/include)

# 用来使用指定的源文件向工程中添加一个目标库
add_library(minicrt STATIC
    entry.c
    format.c
    malloc.c
    stdio.c
    string.c
    cxxabi.cpp
    new_delete.cpp
    string.cpp
    iostream.cpp)

# 创建一个可执行文件
add_executable(test 
        $<TARGET_OBJECTS:minicrt>
        test.cpp)
