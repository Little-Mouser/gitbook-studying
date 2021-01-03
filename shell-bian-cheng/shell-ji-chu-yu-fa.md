# Shell基础语法

## 什么是shell

Shell 是一个应用程序，它连接了用户和 Linux 内核，让用户能够更加高效、安全、低成本地使用 Linux 内核。shell是一个命令解释器，shell本身也是用c语言编写的，对于kernel的操作也是c语言实现的，使用shell可以方便进行一些系统内核操作，而不需要编写复杂的c语言代码去实现。shell是脚本语言，解释执行，c语言需要编译。

unix的哲学：一条命令只做一件事。组合多个命令、多次执行并保存好的文件就是shell脚本。shell脚本执行需要赋予 u=rx 的权限

shell基本格式与执行

首行指定使用的bash类型：\#!/bin/bash （Sha-Bang）

执行方式

* bash  ./test.sh
* ./test.sh
* source ./test.sh
* . ./test.sh

前两种方式会创建子进程，脚本在子进程中执行，执行完成以后返回父进程，不影响父进程环境

后两种方式是在当前进程执行，可能会影响当前进程环境。shell内建命令不会创建子进程，会影响到当前环境，外部命令会创建子进程，不影响当前环境

## 管道符

管道符和信号一样，是进程通信的一种方式。管道符 “\|” 是匿名管道。

管道符基本原理（个人理解）：执行管道操作时，当前进程会 创建一个pipe，同时为管道符左右两边的命令创建子进程（外部命令shell会创建子进程，但是内部命令不会，因此应该避免在管道中使用内部命令），左边子进程的输出连接到管道输入数据，右边子进程的输入连接到管道获取数据

## linux输入输出与重定向

linux中有三种标准输入输出，分别是STDIN，STDOUT，STDERR，对应的数字是0，1，2。

STDIN是标准输入，默认从键盘读取信息；

STDOUT是标准输出，默认将输出结果输出至终端；

STDERR是标准错误，默认将输出结果输出至终端。

### 重定向语法

参考：[https://www.runoob.com/linux/linux-shell-io-redirections.html](https://www.runoob.com/linux/linux-shell-io-redirections.html)

#### 输出重定向：

模式1：覆盖

echo 123 &gt; tt.txt

echo 123 2&gt; tt.txt

echo 123 &&gt; tt.txt

模式2: 追加

echo 123 &gt;&gt; tt.txt

echo 123 2&gt;&gt; tt.txt

echo 123 &&gt;&gt; tt.txt

模式3：stdout和stderr之间重定向

echo 123 &gt; tt.txt  2&gt;&1  表示stdout和stderr都重定向到tt.txt

模式4：重定向到/dev/null，不留日志

echo 123 &gt; /dev/null

#### 输入重定向：

模式1 ： 

cat &lt; tt.txt

模式2：Here Document。一般用于在shell脚本中创建一个文件并赋予内容。

它的基本的形式如下：

```text
command << delimiter
    document
delimiter
```

它的作用是将两个 delimiter 之间的内容\(document\) 作为输入传递给 command

例子：

```text
cat > tt.txt << EOF
i am $user
EOF
```

### 　几种快速清空文件内容的方法：

　　$ : &gt; filename \#其中的 : 是一个占位符, 不产生任何输出.

　　$ &gt; filename

　　$ echo “” &gt; filename

　　$ echo /dev/null &gt; filename

　　$ echo &gt; filename

　　$ cat /dev/null &gt; filename

