## 编译
```
gcc -c test.c
gcc -o main test.o
gcc -Wall -c test.c //-Wall 打印更详细的编译信息
gcc sin.c -lm -L/lib -L/usr/lib  //-l表示加入某个函数库 m 指libm.so函数库 -L表示到目录/lib寻找函数库 -I/usr/include 表示设置lib库目录为/usr/include
```
## 什么是make

软件开发有个检测程序 config 或者 configure ，此程序会生成makefile文件。
检测程序执行目的：

检测合适的编译程序来编译软件
检测合适的函数库和其他相关软件
检测系统平台及内核版本
检测内核的头定义文件

make也是一个程序，在当前目录下寻找makefile文件执行，makefile主要记录源码如何编译的详细信息。

## makefile的基本语法和变量。

## makefile的基本规则

目标(target): 目标文件1 目标文件2
<tab>  gcc -o 欲新建的可执行文件 目标文件1 目标文件2

第一行的目标文件指 object files ，目标指我们要建立的信息(?)
建立可执行文件的语法必须以<tab>开头，在命令行的第一个字符。
目标与相关文件用：隔开。

例子：
main.c haha.c sin_value.c cos_value.c
```
vim makefile:

main: main.o haha.o sin_value.o cos_value.o
      gcc -o main main.o haha.o sin_value.o cos_value.o -lm

clean:
      rm -f main main.o haha.o sin_value.o cos_value.o
```
如上所示，我们的makefile有两个目标。分别是 main 和 clean
根据makefile
如果我们要想执行main创建main目标文件： make main 即可
如果想清除main 执行make clean 即可。
如果想先清除在创建main的话，就makeclean main即可

充分利用shell script 的变量：
1 LIBS= -lm
2 OBJS= main.o haha.o sin_value.o cos_value.o

简化后的makefile:

```bash
LIBS = -lm # 语法1 语法2 语法3
OBJS = main.o haha.o sin_value.o cos_value.o
main: ${OBJS} # 语法4 语法5
    gcc -o ${OBJS} ${LIBS}
clean
    rm -f $@ ${OBJS} #$@代表目前的目标
```


### makefile的基本语法
1 变量与变量的内容以 = 号隔开，同时两边具有空格
2 变量的左边不可以有<tab>
3 变量与变量内容在 = 号两边不能具有 :
4 变量习惯大写字母
5 用${},$()符号引用变量
6 在该shell的环境变量是可以套用，可以用CFLAGS变量。
7 可以自定义变量

makefile编译选项：CFLAGS、CPPFLAGS、LDFLAGS和LIBS的区别

LIBS：已经知道代表连接的库
LDFLAGS : 表示用于 C++ 编译器的选项。 代表连接器选择库的路径 LDFLAGS = -L/var/xxx/lib -L/opt/mysql/lib
CFLAGS：表示用于 C 编译器的选项，指定头文件（.h文件）的路径，如：CFLAGS=-I/usr/include -I/path/include。同样地，安装一个包时会在安装路径下建立一个include目录，当安装过程中出现问题时，试着把以前安装的包的include目录加入到该变量中来。
CXXFLAGS 表示用于 C++ 编译器的选项。

### make命令环境变量的优先级
命令行后的环境变量> makefile中的环境变量 > shell 原本具有的环境变量
