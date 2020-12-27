# 系统管理篇

## 网络状态查看、设置工具



1）net-tools

* ifconfig
* route
* netstat

    2）iproute2

* ip
* ss

### net-tools系列

#### ifconfig（可以单独查一个网卡）

[ifconfig参考](https://blog.csdn.net/qian_xia_er/article/details/79950955)

从centos7开始，系统默认的网卡命名有已经不是我们熟悉的ethX方式了，网卡命名会根据网卡的硬件信息，插槽位置等有关； 例如： 

eno1： 板载网卡 

ens33：PCI-E网卡 

enp0s3：无法获取物理信息的PCI-E网卡 

centOS 7使用了一致性网络设备命名，以上都不匹配则使用eth0

![](.gitbook/assets/image%20%281%29.png)

![](.gitbook/assets/image%20%287%29.png)

#### 更改网卡名称显示规则

比如依然想要网卡名称显示为ethX的形式：

1. 编辑/etc/default/grub文件,增加biosdevname=0和net.ifnames=0
2. 更新grub：grub2-mkconfig -o /boot/grub2/grub.cfg 
3. reboot重启

![](.gitbook/assets/image%20%289%29%20%281%29.png)

规则：

![](.gitbook/assets/image%20%283%29.png)

#### 用mii-tool和ethtool 查看网线是否正确连接到网卡

![](.gitbook/assets/image%20%284%29.png)

#### route -n查看网关

网关为0.0.0.0指不需要网关，与ip对应机器直连

![](.gitbook/assets/image%20%285%29.png)

#### net-tools修改网络配置

            1）启关网卡

* * ifconfig eth0 up
  * ifup eth0
  * ifconfig eth0 down 
  * ifdown eth0

                2） 配置ip地址

* * ifconfig eth0 192.168.1.56 netmask 255.255.255.0 broadcast 192.168.1.255
  * ifconfig eth0 192.168.1.56

                3）修改默认网关路由配置                   

                      route add default gw 192.168.1.1 eth0                   

                       route del default gw 192.168.1.1 eth0                

                4）修改指定网段                    

                       route add -net 192.168.0.0 netmask 255.255.255.0 gw 10.211.55.1                

                 5）修改指定ip路由                    

                        route add -host 10.0.0.1 gw 10.211.55.1

![](.gitbook/assets/image%20%2820%29.png)

![](.gitbook/assets/image%20%286%29.png)

![](.gitbook/assets/image%20%2817%29.png)

## 进程管理

### 进程的概念

![](.gitbook/assets/image%20%2815%29.png)

### 查看进程状态

#### ps命令

ps不带参数，只显示当前终端运行的进程

ps -eLf 可以查看所有进程，以及对应的线程数

![](.gitbook/assets/image%20%2816%29.png)

#### pstree：查看进程树结构

#### top命令：

按数字1，可以统计每个逻辑cpu占用情况，再按1则返回默认

![](.gitbook/assets/image%20%2819%29.png)

内容解读：[https://www.cnblogs.com/peida/archive/2012/12/24/2831353.html](https://www.cnblogs.com/peida/archive/2012/12/24/2831353.html)

load average解读：[https://www.ruanyifeng.com/blog/2011/07/linux\_load\_average\_explained.html](https://www.ruanyifeng.com/blog/2011/07/linux_load_average_explained.html)

top -p查看特定进程实时状态

#### nice、renice控制进程优先级

nice可以控制NI，NI会影响PR（决定优先级），但是PR除了人为控制部分，还有内核自动调节的部分

  **Nice值**的**范围**是-20~+19，拥有**Nice值**越大的进程的实际优先级越小，默认的**Nice值**是0

启动时设置优先级：nice -n 10 ./a.sh

已启动的进程改变优先级： renice -n 15 pid

#### jobs、bg、fg任务前后台切换

1、启动任务时，让一个任务在后台运行 nohup 命令 &

2、已经启动的任务，运行在前台，不想要直接关掉，可以暂时调整为stop状态，放在后台挂起。ctrl +   z

3、jobs可以查看当前shell所有后台任务编号。如果一个后台任务（无论running还是stop），想要在后台run起来，则bg jobid；该后台任务如果想调到前台来执行，则fg jobid

#### kill 使用信号方式进行进程通信

kill -l查看所有信号

![](.gitbook/assets/image.png)

ctrl-c 发送 SIGINT 信号（kill -2）给前台进程组中的所有进程。常用于终止正在运行的程序

kill -9 强行杀死



