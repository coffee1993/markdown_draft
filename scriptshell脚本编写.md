## script shell 脚本编写

### 文件权限
可读可执行

### 执行顺序
从上到下，从左往右，依次执行。

### 命令的执行
忽略命令和参数之间的多个空白

### <CR> Enter
读取到enter就是执行，可以通过\Enter来扩展一行内容

### \# 号作为注释


### 脚本的执行

修改权限：

chmod +x 脚本

绝对路径执行
相对路径执行
变量PATH执行，放入~/bin
bash进程来执行 bash shell.sh 或者sh shell.sh 同时可以用 sh -n 和 sh -x 检测语法错误。

### sh 和bash的区别：

如今Debian和Ubuntu中，/bin/sh默认已经指向dash，这是一个不同于bash的shell，它主要是为了执行脚本而出现，而不是交互，它速度更快，但功能相比bash要少很多，语法严格遵守POSIX标准

主要区别：

1 dash中没有function这个关键字


2 bash支持 select var in list; do command; done
  dash需要while+read+case实现。

3 bash支持echo {0..10} {n,m}展开
  ```bash
  $ echo $0
  /bin/bash
  $ echo {0..10}
  0 1 2 3 4 5 6 7 8 9 10

  dash
  $ echo $0
  dash
  $ echo {0..10}
  {0..10}
  $ echo `seq 0 10`
  0 1 2 3 4 5 6 7 8 9 10
  ```
  ...转 http://www.linuxfly.org/post/686/

### 脚本结构
1 shell 声明
```bash
  #!/bin/bash
```
2 程序说明

3 主要环境的声明
```bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
```
4 主要程序部分

5 告知执行结果



### 交互式脚本
```bash
read -p "please input your filename" filename # 读入用户输入到filename
filename=${fileuser:-filename} # 分析文件名是否有设置。 见鸟哥 Linux 变量内容的替换查找删除 - 代表未设置替换  :- 代表不管变量内容为空或者未设置都以后面的内容替换。
```
