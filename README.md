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
   
   
