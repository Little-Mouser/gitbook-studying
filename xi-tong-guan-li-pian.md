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

![](.gitbook/assets/image%20%2815%29.png)

![](.gitbook/assets/image.png)

![](.gitbook/assets/image%20%286%29.png)

