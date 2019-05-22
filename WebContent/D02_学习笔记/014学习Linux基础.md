# Linux的介绍

概述：

​	linux是一个开源、免费的操作系统，其稳定性、安全性、处理多并发已经得到业界的认可，目前许多企业级的项目都会部署到linux/unix系统上

创始人： 

​	Linus Torvalds 林纳斯

Linux主要的发行版：

​	Ubuntu(乌班图)、RedHat(红帽)、CentOS、Debain(蝶变)、Fedora、SuSE、OpenSUSE

虚拟机网络连接的三种模式：

​	桥接模式：

​		1、好处是，大家都在同一个网断，相互可以通信

​		2、缺点是，因为ip地址有限，可能造成ip冲突

​	NAT模式（网络地址转换模式 Network Address Transfer）：

​		1、好处是，虚拟机不占独占ip，所以不会ip冲突

​		2、缺点是，内网的其他用户不能和虚拟机通讯

​	仅主机模式：

​		1、就是一台单独的电脑

# Linux配置网络

```shell
#进入文件
cd /etc/sysconfig/network-scripts/
sudo vi ifcfg-ens33

#修改ip
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.92.101
GATEWAY=192.168.92.2
DNS1=192.168.92.2

#重启网卡
service network restart

#查看网络
ifconfig
```

# Linux的目录结构

##### 基本的介绍

​	Linux的文件系统是采用级层式的树状目录结构，在此结构中的最上层是根目录"/"，然后在此目录下再创建其他的目录。

​	记住一句经典的话：在Linux世界里，一切皆文件（即使是一个硬件设备，也是使用文本来标志）



# Linux关闭防火墙命令

```sql
# 在外部访问CentOS中部署应用时，需要关闭防火墙。
# 查看防火墙状态
firewall-cmd --state

# 开启、关闭firewall
systemctl start firewalld.service
systemctl stop firewalld.service

# 设置firewall开机启动
systemctl disable firewalld.service 
systemctl enable firewalld.service
```













视频 08/53