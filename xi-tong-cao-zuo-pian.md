# 系统操作篇

## 帮助命令

### man --manual 手册



man根据章节来获取内容，命令为 man 7 ls（q退出）；帮助手册章节总共为9章，不同的命令放在不同章节，如果出现命令重名，则可以按照章节区分出来，默认按照章节顺序搜索，某个章节搜索到则返回结果。-a参数可以搜索所有章节

![](.gitbook/assets/image%20%2812%29.png)

### help命令

type 命令可以判断命令的类型

![](.gitbook/assets/image%20%2811%29.png)



type命令用来显示指定命令的类型。一个命令的类型可以是如下之一

* alias 别名
* keyword 关键字，Shell保留字
* function 函数，Shell函数
* builtin 内建命令，Shell内建命令
* file 文件，磁盘文件，外部命令
* unfound 没有找到

type -a可以显示所有可能的类型，比如有些命令如pwd是shell内建命令，也可以是外部命令。

type -p只返回外部命令的信息，相当于which命令（会显示命令对应文件路径）。

type -f只返回shell函数的信息。

type -t 只返回指定类型的信息。

help分为内部命令\(shell自带\)和外部命令（可以先使用type区分命令类型），用法分别为 

help ls

ls --help 

### info命令

info是help的补充，用法如下 info cd

#### which 查找可执行文件的位置

####  ctrl+r 搜索已经使用过的命令

## 文件操作

#### cd 目录切换 

cd 绝对路径/  ./  ../

#### ls 查看当前目录下的文件

常用参数：

* -l 长格式显示文件
* -a 显示隐藏文件
* -r 逆序显示（默认情况下，ls是以文件名称进行显示排序的）
* -t 按照时间顺序显示
* -R 递归

ls指定特定文件目录（可以指定多个，如果没有指定则文件路径参数为.）

![](.gitbook/assets/image%20%282%29.png)

#### pwd 显示当前目录

#### mkdir rmdir rm 创建目录

不能创建已存在的目录

mkdir -p /aa/bb /aa/cc 创建多级、多个目录

rmdir不能删除多级或非空目录

rm -rf 删除多级、非空目录（rm /aa/cc与 rm /aa/cc/效果相同，都会把cc以及子目录全部删除）

####  cp \[options\] source dest 复制文件或目录 

cp -r ./dir1/ ./dir2/与cp -r ./dir1 ./dir2/不一样，前者是复制dir1的子目录、文件，后者是连带dir1一起复制

常用参数：

-a：同时指定"-dpR"； 

-d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录； 

-p：保留源文件或目录的属性，包含权限、属主、属组、时间戳等； -R/r：递归处理，将指定目录下的所有文件与子目录一并处理，不加参数则只能复制单个文件

通配符： cp -v ./file\* /（针对文件的操作，文件名都可以使用通配符表示，\*代表多个字符，？代表单个字符） 

#### mv 移动和重命名文件或目录

移动：mv ./aa ./cc

重命名：mv ./cc ./dir1/

移动并重命名：mv ./dir1/cc ./dir2/dd

针对目录的操作，mv ./dir2 /tmp/dir3如果/tmp/dir3存在，则出现新目录/tmp/dir3/dir2，仅仅只是移动，否则为改名dir3

#### cat head tail wc more less文本查看

cat：文本内容显示到终端

more ：

head：查看文件开头

            默认显示10行，可以指定行数 head -n

tail：查看文件结尾

          -f：实时

         -n：默认显示十行，可以指定数字

wc：统计文件内容，默认显示文件的字节数、行数、字数

        -c ： 显示Bytes数。

        -l： 显示行数。

        -w：显示字数

more：按页查看文件

         空格键向下翻页；b键向上翻页；q退出

less：在more的基础上，增加查询功能

* -N 显示每行的行号 
* 空格键 滚动一页 
*  b 向后翻一页 
*  /字符串：向下搜索“字符串”的功能 
* ?字符串：向上搜索“字符串”的功能 
* n：重复前一个搜索（与 / 或 ? 有关），并按整页显示 
* N：反向重复前一个搜索（与 / 或 ? 有关），并按整页显示

#### tar gzip bzip2 打包和压缩

[tar命令参考](https://www.cnblogs.com/starof/p/4229017.html)

tar用于把文件打成tar包，gzip等将文件压缩成gz包，如果gzip去压缩tar包，则文件后缀为.tar.gz或.tgz；若使用bzip2压缩的话，则后缀为.tar.bz2或./tbz2。使用tar命令时，可以增加参数直接集成压缩

涉及打包的才使用tar命令，直接压缩或者解压缩gz文件直接使用gzip和ungzip命令

gzip速度更快，bzip体积更小

1）主选项：【一条命令以下5个参数只能有一个】

-c: --create 新建一个压缩文档，即打包

-x: --extract,--get解压文件

-t: --list,查看压缩文档里的所有内容

-r:--append 向压缩文档里追加文件

-u:--update 更新原压缩包中的文件

2）辅助选项：

-z:是否同时具有gzip的属性？即是否需要用gzip压缩或解压？一般格式为xxx.tar.gz或xx.tgz

-j：是否同时具有bzip2的属性？即是否需要用bzip2压缩或解压？一般格式为xx.tar.bz2

-v:显示操作过程！这个参数很常用

-f：使用文档名，注意，在f之后要立即接文档名，不要再加其他参数！

-C:切换到指定目录

--exclude FILE:在压缩过程中，不要将FILE打包

3\)常见示例

    tar -zcvf img.tar.gz img1 img2  打包并压缩

     tar -zxvf img.tar.gz 解压

      tar -jtvf img.tar.bz2 查看压缩包内容

4）解压命令总结

1、\*.tar 用 tar –xvf 解压

2、\*.gz 用 gzip -d或者gunzip 解压

3、\*.tar.gz和\*.tgz 用 tar –xzf 解压

4、\*.bz2 用 bzip2 -d或者用bunzip2 解压

5、\*.tar.bz2用tar –xjf 解压

6、\*.Z 用 uncompress 解压

7、\*.tar.Z 用tar –xZf 解压

8、\*.rar 用 unrar x 解压

9、\*.zip 用 unzip 解压

## vi vim 文本编辑器

1\)四种模式

模式转换：刚进入vi时是正常模式，模式切换时，使用Esc可以从其他模式切换到正常模式，然后再切换到其他模式

a. 正常模式切换到插入模式

      小写i：光标位置不动

       小写a：光标移动到后一位

       大写A：光标移动到行末

       小写o：向下新建空行，光标移动到空行首位

       大写O：向上新建空行，光标移动到空行首位

b.正常模式切换到命令模式 ：冒号

 2）正常模式 Normal-Mode

       光标上下左右移动：KJHL键

      光标移动到文件首行：g

       光标移动到文件末行：G

      光标移动到特定行数：num + G

      光标移动到行首：^（shift + 6）

      光标移动到行尾：$（shift+4）

       粘贴：p

       复制：yy：复制一行，如果复制3行则为3yy，粘贴时从光标的下一行开始粘贴

                   y$：从光标位置复制到行末，粘贴时从光标位置向后粘贴

                   yw：复制一个word（默认word间以空格隔离），复制3个word则为y3w

                  yG：复制到文件末

                  y1G：复制到文件首

       剪切：dd、d$、dw、dG、d1G，用法与复制类似

       删除单个字符：x

       替换单个字符：r 然后输入新值

       撤销上一步操作：u（例如剪切错误，需要复原）

       重复执行上一步操作：ctrl + r

       显示文件行数：:set nu

3）插入模式 Insert-mode

4）命令（末行）模式 command-mode 

退出：q

保存 ：w

临时执行shell命令：  ！后面跟命令，执行完以后Enter键返回（例如:!config） 

查找和替换：

      /keyword ：向光标下搜索keyword字符串，keyword可以是正则表达式 

      ?keyword ：向光标上搜索keyword字符串 

       n ：正向查找下一个元素（向上还是向下由/ 或？决定）

        N ：逆向查找下一个元素（向上还是向下由/ 或？决定）

        :s/old/new ：用new替换单行中首次出现的old 

        :s/old/new/g ：用new替换单行中所有的old 

        :n,m s/old/new/g ：用new替换从n到m行里所有的old 

       : %s/old/new/g ：用new替换文件里所有的old

设置：

        显示与不显示行号：:set nu      :set nonu

        要想vi的设置永久生效，需要编辑vi的配置文件。vim /etc/vimrc 并在最后一行添加set nu

5）可视模式 Visual-mode 

三种可视模式：

* v ：字符可视模式（以单个字符为单位）
* V :  行可视模式（以行为单位）
* ctrl + v ：块可视模式（使用最多）

插入文本时，填写完插入的文本，需要双击ESC

![](.gitbook/assets/image%20%288%29.png)







