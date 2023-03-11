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




```





