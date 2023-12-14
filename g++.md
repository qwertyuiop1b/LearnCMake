### 生成可执行程序
#### 预处理：宏定义替换、#include头文件、删除注释等
```cmd
-E =>  Preprocess only; do not compile, assemble or link

g++ -E main.cpp -o main.i

```
#### 编译: 生成汇编代码
```cmd
-S => Compile only; do not assemble or link

g++ -S main.cpp  // 默认生成main.s文件

```

#### 汇编：汇编代码转成可执行指令
```cmd
-c => Compile and assemble, but do not link

g++ -c main.cpp // 默认生成main.o文件

```

#### 链接: 链接静态库和动态库
```cmd
-o 生成可执行程序

g++ -o main.cpp main

```

### 静态库制作
- 静态库其实把库的.o文件打包成一个整体
```cmd
// 示例：制作hmath静态库
─ hmath
│   ├── include
│   │   └── hmath.h
│   ├── src
│   │   ├── add.cpp
│   │   └── sub.cpp
// hmath.h
int add(int a, int b);
int sub(int a, int b);

// add.cpp
#include "../include/hmath.h"

int add(int a, int b) {
  return a + b;
}

// sub.cpp
#include "../include/hmath.h"

int sub(int a, int b) {
  return a - b;
}

=========================================
g++ -c ./src/sub.cpp ./src/add.cpp 
ar rcs libhmath.a add.o sub.o 


// main.cpp 
// -I${include_path}(指定头文件路径)  -L${lib_path}（指定查找库的路径） -l${lib_name}（指定库名称）
g++ main.cpp -I./  -L./ -lhmath -o main


```

### 动态库制作

```cmd
// -fPIC (position independent code) 告诉编译器生成与地址无关的代码
g++ -fPIC -c ./src/sub.cpp ./src/add.cpp
g++ -shared -o lib${lib_name}.so sub.o add.o
合并一起：
g++ -fPIC -shared -o lib${lib_name}.so ./src/sub.cpp ./src/add.cpp

// 执行的话：
export LD_LIBRARY=$LD_LIBRARY_PATH:${lib_path}:${lib_path1}:${lib_path2}

如果$LD_LIBRARY_PATH为空的话；
if [-z "$LD_LIBRARY_PATH"]; then
  export LD_LIBRARY_PATH=${path_name}
else 
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/${path_name}
fi  

```

