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
    
  等目录,那么/home/xxx/gcc-linaro-7.4.1-2019.02-x86_64_aarch64-linux-gnu/aarch64-linux-gnu/libc/usr亦可作为sysroot目录，指定了sysroot目录后如之前会在/usr/lib下寻找
     
     
