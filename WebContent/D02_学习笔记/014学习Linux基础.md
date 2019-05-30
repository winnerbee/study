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





```
su 和 su -区别
su 切换用户，不切换环境变量
su - 切换用户，切换环境变量

```

# CentOS更换国内yum源（aliyun阿里镜像）

```sh
1、下载aliyun的repo

[root@master ~]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

--2016-12-30 23:33:00--  http://mirrors.aliyun.com/repo/Centos-7.repo
Resolving mirrors.aliyun.com... 112.124.140.210, 115.28.122.210
Connecting to mirrors.aliyun.com|112.124.140.210|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2573 (2.5K) [application/octet-stream]
Saving to: “/etc/yum.repos.d/CentOS-Base.repo”

100%[=======================================================================>] 2,573       --.-K/s   in 0s      

2016-12-30 23:33:00 (147 MB/s) - “/etc/yum.repos.d/CentOS-Base.repo” saved [2573/2573]

2、更新yum缓存

[root@master ~]# yum clean all

Loaded plugins: fastestmirror, refresh-packagekit, security
Cleaning repos: base extras updates
Cleaning up Everything
Cleaning up list of fastest mirrors
[root@master ~]# yum makecache
Loaded plugins: fastestmirror, refresh-packagekit, security
Determining fastest mirrors
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
base                                                                                                                                                 | 3.7 kB     00:00     
base/group_gz                                                                                                                                        | 226 kB     00:00     
base/filelists_db                                                                                                                                    | 6.4 MB     00:06     
base/primary_db                                                                                                                                      | 4.7 MB     00:04     
base/other_db                                                                                                                                        | 2.8 MB     00:02     
extras                                                                                                                                               | 3.4 kB     00:00     
extras/filelists_db                                                                                                                                  |  38 kB     00:00     
extras/prestodelta                                                                                                                                   | 1.3 kB     00:00     
extras/primary_db                                                                                                                                    |  37 kB     00:00     
extras/other_db                                                                                                                                      |  51 kB     00:00     
updates                                                                                                                                              | 3.4 kB     00:00     
updates/filelists_db                                                                                                                                 | 2.5 MB     00:02     
updates/prestodelta                                                                                                                                  | 279 kB     00:00     
updates/primary_db                                                                                                                                   | 3.7 MB     00:03     
updates/other_db                                                                                                                                     |  49 MB     00:49   

Metadata Cache Created
```







视频 08/53