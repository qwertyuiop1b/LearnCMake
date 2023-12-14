### Makefile 
#### 入门
- hello world 示例
  ```make
  # make必须使用制表符缩进，不能使用空格
  hello:
    echo "Hello World"
  ```
  
- 生成文件语法
  ```make
  targets: prerequisites
    command
    command
    command
  ```
  - 目标是文件名，以空格隔开，每条规则只有一个
  - 命令以制表符开头，不能空格
  - 先决条件也是文件名，以空格隔开

- Make的本质
  ```make
  hello:
    echo "Hello World"
    echo ""This line will print if the file hello does not exits."
  ```
  目标为hello，两条命令，没有先决条件；如果make，hello文件不存在的话，会打印这两条语句；hello存在的话，就不会执行任何命令

  创建如下的两个文件：
  ```c
  // blah.c
  int main () {return 0;}
  ```
  ```make
  # Makefile 
  blah:
    cc blah -o blah
  ```
  执行make命令执行，生成blah文件；如果继续执行的话，blah已是最新，但是修改blah.c，之后重新make，不会重新编译任何内容

  解决方法：
  ```make
  # Makefile
  blah: blah.c
    cc blah.c -o blah
  ```
  - 选择第一个目标，因为第一个目标是默认目标
  - 前提条件：存在blah.c
  - Make是否运行blah目标，取决于blah.c或者blah.c是否更新 
  
- 更快速的示例
  ```make
  # Makefile
  blah: blah.o
    cc blah.o -o blah 
  
  blah.o: blah.c
    cc -c blah.c -o blah.o
  
  blah.c:
    echo "int main() {return 0;}" > blah.c
  ```
  blah.c目标没有先决条件，若存在blah.c,不会执行echo "int main() ...", 但是手动更改blah.c的内容，blah.o的目标执行，blah也继续执行，依次类推...
  
- 清理
  ```make
  some_file: 
    touch some_file
  
  clean:
    rm -rf some_file
  ```
  make默认执行第一个目标，所以执行touch some_file; make clean 执行rm -rf some_file; clean并不是Make中的特殊的词
  
- 变量
  
  变量只能是字符串，:= 或者 =；示例如下：
  
  ```make
  files := file1 file2
  some_file: $(files)
    echo "Look at this variable: " $(files)
    touch some_file
  
  file1:
    touch file88
  
  file2:
    touch file99
  
  clean: 
    rm -rf file88 file99 some_file
  ```
  单引号或双引号对Make没有任何意义。它们只是分配给变量的字符，不过引号对于shell、bash有用
  ```make
  a := one two # a is set to the string "one tow"
  b := 'one two' # Not recommended. b is set to the string "'one two'"
  
  all: 
    printf '$a'
    printf $b
  ```
  
  ```make
  string := hello world
  
  printMessage: 
    echo ${string}
    echo $(string)
  
    # Bad practice, but works
    echo $string
  ```
### 目标
- 全部目标
  创建多个目标并希望所有目标运行
  ```make
  all: one two three
  
  one:
    touch one
  
  two:
    touch two
  
  three:
    touch three
  
  clean:
    rm -rf one two three
  ```
- 多个目标
  当规则多个目标时，将为每个目标运行命令，```$@```是包含目标名称的自动变量
  ```make
  all: f1.o f2.o
  
  f1.o f2.o:
    echo $@
  # f1.o:
  #     echo f1.o
  # f2.o:
  #     echo f2.o
  ```
### 自动变量和通配符
- ```*```通配符
  ```make
  # print out file information about every file
  print: $(wildcard *.c)
    ls -la $?
  ```
  可以在目标、先决条件或者函数中使用wildcard, ```*```不能直接在变量定义中使用，当```*```没有匹配任何文件时，将保持原样（除非在wildcard中使用）

- ```%```通配符
- 自动变量
  ```make
  all: one two
    # 输出目标名字
    echo $@
    # 输出比target新的所有先决条件
    echo $?
    # 输出所有先决条件
    echo $^
  ```
### 规则
- 隐含规则
- 静态模式规则
- 静态模式规则和过滤器
- 模式规则
- 双冒号规则
  



