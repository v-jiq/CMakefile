# CMakefile

## cross-compile:
``` cmake
#You must configure this to use cross compilation
SET(CMAKE_SYSTEM_NAME Linux)

#Specify the C cross - compilation tool
SET(CMAKE_C_COMPILER "arm-oe-linux-gnueabi-gcc")

#Specify the C++ cross - compilation tool
SET(CMAKE_CXX_COMPILER "arm-oe-linux-gnueabi-g++")

#CMAKE_SYSROOT is only valid after version 3.0
SET(CMAKE_SYSROOT "/home/jjiq/neoway/N720A_OPEN_LINUX_Q201_SDK_V3.02/tool/neoway-arm-oe-linux/sysroots/armv7a-vfp-neon-oe-linux-gnueabi")

#Tell the compiler where to find the library
SET(CMAKE_FIND_ROOT_PATH "/home/jjiq/neoway/N720A_OPEN_LINUX_Q201_SDK_V3.02/tool/neoway-arm-oe-linux/sysroots/armv7a-vfp-neon-oe-linux-gnueabi")

SET (CMAKE_C_COMPILER_WORKS 1)

SET (CMAKE_CXX_COMPILER_WORKS 1)

#Never look for utility programs in the specified directory
#SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

#Look for library files only in the specified directory
SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)

#Only look for header files in the specified directory
SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
```

其中：   
     
     CMAKE_FIND_ROOT_PATH   
     
     CMAKE_FIND_ROOT_PATH_MODE_PROGRAM 
     
     CMAKE_FIND_ROOT_PATH_MODE_LIBRARY  
     
     CMAKE_FIND_ROOT_PATH_MODE_INCLUDE  
     
这几项应该是搭配使用的,CMAKE_FIND_ROOT_PATH一般是用于设置为交叉编译工具链中的sysroot目录,若是有些交叉编译工具链中无此目录,则可找寻目录下包含usr、include、bin等根目录下常见的目录的目录作为sysroot目录    
  如:  
  
   在/home/xxx/gcc-linaro-7.4.1-2019.02-x86_64_aarch64-linux-gnu/aarch64-linux-gnu/libc/usr目录下有  
    
    ```shell
    bin  include  lib  libexec  sbin  share
    ```
    
  等目录,那么/home/xxx/gcc-linaro-7.4.1-2019.02-x86_64_aarch64-linux-gnu/aarch64-linux-gnu/libc/usr亦可作为sysroot目录，指定了sysroot目录后如之前会在/usr/lib下寻找目录，那么在设置了sysroot目录后就会在sysroot/usr/lib下寻找.  
  
  而CMAKE_FIND_ROOT_PATH_MODE_LIBRARY、CMAKE_FIND_ROOT_PATH_MODE_INCLUDE则是用于标识编译工具链是否只在由CMAKE_FIND_ROOT_PATH指定的目录下寻找库和头文件. 而CMAKE_FIND_ROOT_PATH_MODE_PROGRAM则标识是否在由CMAKE_FIND_ROOT_PATH指定的目录下寻找编译的工具,若是交叉编译此项一般为NEVER,因为一般的交叉编译工具链的编译工具链都不会放在sysroot目录中,编译工具都由CMAKE_C_COMPILER和CMAKE_CXX_COMPILER项来指定,且一般会把目录设置在环境变量中.
  
  ```cmake
  CMAKE_SYSROOT
  ```
  该项的用法跟CMAKE_FIND_ROOT_PATH很相似,都是指定ROOT目录用于寻找库和头文件等,一般这两项的目录在交叉编译过程中都会设置为一样.暂时还没搞清这两者的区别.  
   ```cmake 
   CMAKE_SYSTEM_NAME
   ```
   若是交叉编译此项必设为linux.  
   
   ```cmake 
   CMAKE_C_COMPILER_WORKS  
   CMAKE_CXX_COMPILER_WORKS
   ```
   
   cmake在生成Makefile时会检测编译工具链是否可用,但有时此步骤会出错，但实际上工具链是可用的,即可使用这两项免于检测.
   
   使用该.cmake文件用于交叉编译 cmake -DCMAKE_TOOLCHAIN_FILE=xxx.cmake .即可
   

   
## CMakeLists.txt的基本用法:
  ```cmake
cmake_minimum_required(VERSION 3.2.0)
add_definitions(-std=c++11)
add_definitions(-g -o2)

#define cuda,opencv,cudnn
ADD_DEFINITIONS( -DGPU -DCUDNN )

# use opencv
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} "/usr/local/share/OpenCV")
find_package(OpenCV 3.2.0 REQUIRED)

if(NOT OpenCV_FOUND)
        message(WARNING "OpenCV not found!")
else()
        include_directories(${OpenCV_INCLUDE_DIRS})
endif()

# CUDA path
include_directories(/usr/local/cuda-8.0/include/ /usr/include)

# headers
include_directories(${PROJECT_SOURCE_DIR}/include)

#sources
set(SRC  ${PROJECT_SOURCE_DIR}/test.cpp)

#lib link
link_directories(${PROJECT_SOURCE_DIR})
link_directories(${OpenCV_LIBRARY_DIRS})

#build so
add_library(plate_recognition SHARED ${SRC})
target_link_libraries(plate_recognition ${OpenCV_LIBS})
target_link_libraries(plate_recognition -lxxx -lxxx  -lpthread -lm -lstdc++)
  ```
 其中
 ```cmake
 CMAKE_PREFIX_PATH  
 find_package
 ```
 一般是一起搭配使用的,find_package用于找寻相应的目标库，在指定和默认的目录中搜寻搜寻相应库的.cmake文件,而CMAKE_PREFIX_PATH项则用于指定cmake的搜索路径.
 
```cmake
message
```
该变量是用于调试CMakeList,即可以打赢变量也可以直接打印常量.

```cmake
option
```
Provides an option that the user can optionally select.
该变量可用于调试CMakelists流程，具体用法可以参考azure-iot-sdk-c的CMakeLists.txt里的用法
 
 
 
