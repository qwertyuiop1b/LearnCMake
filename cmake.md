### CMake Tutorial
- mkdir build cd build
- cmake ${source}
- cmake --build .

#### Step1  Start

```cmake
cmake_minimum_required(VERSION 3.10) 
project(ProjectName)
project(ProjectName VERSION 1.0) # 配置项目版本

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 自定义输入变量
set(FOO hello_world) # 配置文件中的@FOO@文本替换为hello_world

# ProjectNameConfig.h.in头文件，文件ProjectNameConfig.h位于project binary directory
configure_file(ProjectNameConfig.h.in  ProjectNameConfig.h) 

add_executable(ProjectName main.cpp)  // ProjectName可加可不加

target_include_directories(ProjectName PUBLIC "${PROJECT_BINARY_DIR}") // include目录


# 如果c++文件想要读取项目版本信息，配置一个输入文件，例如
# 新建ProjectNameConfig.h.in
# 定义自定义变量
#define ProjectName_VERSION_MAJOR @PROJECTNAME_VERSION_MAJOR@
#define ProjectName_VERSION_MINOR @PROJECTNAME_VERSION_MINJOR@
#define FOO "@FOO@"

=======================================================
ProjectNameConfig.h
#define PROJECTNAME_VERSION_MAJOR 1
#define PROJECTNAME_VERSION_MINJOR 0
#define FOO hello_world
 
 c++ main.cpp
#include "ProjectNameConfig.h"
 
 

```

#### Step2 Adding a library

```cmake
############################################
# Add a library
############################################
# 1. 库目录下的CMakeLists.txt
add_library(${libName} STATIC/SHARED ${buildDir} ${sourceFile}) # 库名称 / 生成的库的输出目录 / 源文件
add_library(mylib STATIC src/mylib.cpp) # 示例

# 指定一个目录
aux_source_directory(src DIR_SRCS)  
add_library(mylib SHARED ${DIR_SRCS})

# 指定多个目录，并合并一个变量中
aux_source_directory(src DIR_SRCS) # 收集目录下的所有源文件并存储到变量中
aux_source_directory(src/subdir SUBDIR_SRCS)
set(ALL_SRCS ${DIR_SRCS} ${SUBDIR_SRCS})
add_library(mylib SHARED ${ALL_SRCS})


# 2. 项目根CMakeList.txt添加子目录到构建中
add_subdirectory(mylib) 


# 3. 项目根CMakeList.txt指定引入库的头文件路径
target_include_directories(${targetName} PUBLIC/PRIVATE "${PROJECT_SOURCE_DIR}/mylib" "${PROJECT_BINARY_DIR}")

#################################################




###################################################
# Make a library optional
##################################################

# 1. 创建一个option
option(USE_MYLIB "option description message" ON/OFF) # 定义一个option选项为ON

# 2. 配置文件：#cmakedefine USE_MYLIB    【#cmakedefine variable value】
#    编译后配置文件 #define USE_MYLIB or #undef USE_MYLIB；可以通过命令行设定, 例如
#    cmake ${SOURCE_DIR} -DUSE_MYLIB=OFF


if(USE_MYLIB)
  add_subdirectory(${mylibPath})
  list(APPEND EXTRA_LIB ${libName})
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/${mylib}")
endif()


```





