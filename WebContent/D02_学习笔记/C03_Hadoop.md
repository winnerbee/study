# 大数据技术之Hadoop（入门）

## 第一章 大数据概论

### 1、大数据概念

大数据（Big Data）：指无法在一定时间范围内用常规工具进行捕捉、管理和处理的数据集合，是需要新处理模式才能具有更强的决策力、洞察发现力和流程优化的海量、高增长率和多样化的信息资产。

一线公司：阿里、腾讯、今日头条

二线公司：美团、滴滴

按顺序给出数据存储单位：bit、Byte、KB、MB、GB、TB、PB、EB、ZB、YB、BB、NB、DB。

```shell
面试题：你谈一谈对大数据的理解？
回答：主要解决，海量数据的存储和海量数据的分析计算问题。
```

### 2、大数据特点(4V)

1、Volume(大量)

​	截止目前，人类生产的所有印刷材料的数据量是200PB,而历史上全人类共说过的话的数量大约是5EB。当前，典型个人计算机硬盘的容量为TB量级，而一些大企业的数据量已经接近EB量级。

2、Velcity(高速)

​	这是大数据区分于传统数据挖掘的最显著特征。根据IDC的"数字宇宙"的报告，预计到2020年，全球数据使用量将达到35.2ZB。在如此海量的数据面前，处理数据的效率就是企业的生命。

3、Variety(多样)

​	这种类型的多样性也让数据分为结构化数据和非结构化数据。相对于以往便于存储的以数据库、文本为主的结构化数据，非结构化数据越来越多，包括网络日志、音频、视频、图片、地理位置信息等，这些多类型的数据对数据的处理能力提出了更高的要求。

4、Value(低价值密度)

​	价值密度的高低与数据总量的大小成反比。比如，在一天的监控视频中，我们只关心宋宋老师晚上在床上健身的那一分钟，如何快速对有价值数据"提纯"成为目前大数据背景下待解决的问题。

### 3、大数据应用场景

1. 物流仓储：大数据分析系统助力商家精细化运营、提升销量、节约成本。
2. 零售：分析用户消费习惯，为用户购买商品提供方便，从而提升商品销量。经典案例，尿布+啤酒。
3. 旅游：深度结合大数据能力与旅游业需求，共建旅游产业智慧管理、智慧服务和智慧销售的未来。
4. 商品广告推荐：给用户推荐可能喜欢的商品。
5. 保险：海量数据挖掘及风险预测，助力保险行业精准营销，提升精细化定价能力。
6. 金融：多维度体现用户特征，帮助金融机构推荐优质客户，防范欺诈风险。
7. 房产：大数据全面助力房地产行业，打造精准投策与营销，选出更合适的地，建造更合适的楼，卖给更合适的人。
8. 人工智能：英文缩写为AI。它是研究、开发用于模拟、延伸和扩展人的智能的理论、方法、技术及应用系统的一门新的技术科学。

## 第二章 从Hadoop框架讨论大数据生态

### 1、Hadoop是什么？

1. Hadoop是一个由Apache基金会所开发的分布式系统基础架构。
2. 主要解决，海量数据的存储和海量数据的分析计算问题。
3. 广义上来说Hadoop通常是指一个更广泛的概念 -- Hadoop生态圈。

### 2、Hadoop三大发行版本

1. Apache：最原始(最基础)的版本，对于入门学习好。
2. Cloudera：在大型互联网企业中用得较多。
3. Hortonworks：文档较好。

### 3、Hadoop的优势（4高）

1. 高可靠性：Hadoop底层维护多个数据副本，所以即使Hadoop某个计算元素或存储出现故障，也不会导致数据的丢失。
2. 高扩展性：在集群间分配任务数据，可方便的扩展数以千计的节点。
3. 高效性：在MapReduce的思想下，Hadoop是并行工作的，以加快任务处理的速度。
4. 高容错性：能够自动将失败的任务重新分配。

### 4、Hadoop组成（面试重点）

![Hadoop1.x和Hadoop2.x区别](大数据md图片\Hadoop1.x和Hadoop2.x区别.png)

#### 1、HDFS架构概述：

名词解释：HDFS全称 Hadoop Distributed File System

1. NameNode（nn）：存储文件的元数据，如文件名，文件目录结构，文件属性（生成时间、副本数、文件权限），以及每个文件的块列表和块所在的DataNode等。
2. DataNode（dn）：在本地文件系统存储文件块数据，以及块数据的校验和。
3. Secondary NameNode（2nn）：用来监控HDFS状态辅助后台程序，每隔一段时间获取HDFS元数据的快照。

#### 2、YARN架构概述：xxxx

#### 3、MapReduce架构概述：xxxx



### 5、大数据技术生态体系xxx



### 6、推荐系统框架图xxx



## 第三章 Hadoop运行环境搭建（开发重点）

### 1、安装jdk

#### 1、删除系统自带jdk

```shell
# 删除jdk
# 查询是否安装java软件
rpm -qa | grep java
sudo rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.161-2.b14.el7.x86_64
sudo rpm -e --nodeps java-1.7.0-openjdk-1.7.0.171-2.6.13.2.el7.x86_64
sudo rpm -e --nodeps java-1.8.0-openjdk-1.8.0.161-2.b14.el7.x86_64
sudo rpm -e --nodeps javapackages-tools-3.4.1-11.el7.noarch
sudo rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.171-2.6.13.2.el7.x86_64
# 注意 python-javapackages-3.4.1-11.el7.noarch、tzdata-java-2018c-1.el7.noarch不要删除
```

#### 2、安装jdk

```shell
# 在opt目录下创建文件夹
cd /opt
sudo mkdir module
sudo mkdir software
# 修改文件夹所有者
sudo chown jay:jay module/ software/ -R

# 解压jdk
cd software/
tar -zxvf jdk-8u181-linux-x64.tar.gz -C /opt/module/
# 注意 -zxvf(解压)  -C(要解压到某个目录)

# 配置jdk环境变量
sudo vim /etc/profile
# 在文件末尾追加
##JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_181
export PATH=$PATH:$JAVA_HOME/bin

# 配置生效
source /etc/profile

# 测试安装是否成功
java -version
```

### 2、安装hadoop

#### 1、Hadoop下载地址：

https://archive.apache.org/dist/hadoop/common/hadoop-2.7.2/

#### 2、安装Hadoop

```shell
# 解压hadoop
cd /opt/software/
tar -zxvf hadoop-2.7.2.tar.gz -C /opt/module/

# 配置hadoop环境变量
sudo vim /etc/profile
# 在文件末尾追加
# HADOOP_HOME
export HADOOP_HOME=/opt/module/hadoop-2.7.2
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

# 配置生效
source /etc/profile

# 测试安装是否成功
hadoop version
```

#### 3、重要目录：

1. bin目录：存放对Hadoop相关服务（HDFS,YARN）进行操作的脚本
2. etc目录：Hadoop的配置文件目录，存放Hadoop的配置文件
3. lib目录：存放Hadoop的本地库（对数据进行压缩解压缩功能）
4. sbin目录：存放启动或停止Hadoop相关服务的脚本
5. share目录：存放Hadoop的依赖jar包、文档、和官方案例

## 第四章 Hadoop运行模式

### 1、本地运行模式

#### 1.1、本地模式-官方Grep案例

```shell
# 准备工作
cd /opt/module/hadoop-2.7.2
mkdir input
cp etc/hadoop/*.xml input/

# 执行
cd /opt/module/hadoop-2.7.2
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar grep input output 'dfs[a-z.]+'
# 注意：output文件夹不能存在，否则会报错FileAlreadyExistsException
```

#### 1.2、本地模式-官方WordCount案例

```shell
# 准备工作
cd /opt/module/hadoop-2.7.2
mkdir wcinput
cd wcinput
touch wc.input
vi wc.input
# 文本中输入内容
hadoop yarn
hadoop mapreduce
atguigu
atguigu

# 执行
cd /opt/module/hadoop-2.7.2
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount wcinput wcoutput

# 查看结果
cd /opt/module/hadoop-2.7.2/wcoutput
vim part-r-00000
```

### 2、完全分布式模式

#### 2.1、准备工作：

1. 准备3台客户机（静态ip、主机名称、root权限、hosts、关闭防火墙）
2. 安装jdk
3. 配置jdk环境变量
4. 安装hadoop
5. 配置hadoop环境变量
6. 配置集群
7. 单点启动
8. 配置ssh
9. 群起并测试集群

#### 2.2、scp（secure copy） 安全拷贝：

```shell
scp定义：scp可以实现服务器与服务器之间的数据拷贝。
基本语法：
scp		-r		$pdir/$fname		$user@hadoop$host:$pdir/$fname
命令    递归	 源文件路径/名称		目标用户@主机:目标路径/名称
```

实例操作：

```shell
# 在hadoop101上，将hadoop101服务器上的/opt/module目录下的软件拷贝到hadoop102上
sudo scp -r /opt/module root@hadoop102:/opt/

# 在hadoop103上，将hadoop101服务器上的/opt/module目录下的软件拷贝到hadoop103上
sudo scp -r root@hadoop101:/opt/module /opt/

# 在hadoop102上，将hadoop101服务器上的/opt/module目录下的软件拷贝到hadoop103上
sudo scp -r root@hadoop101:/opt/module root@hadoop103:/opt/

# 注意：拷贝过来的/opt/module目录，别忘了在hadoop101、hadoop102、hadoop103上修改所有文件的，所有者和所有者组。sudo chown jay:jay -R /opt/module

# 在hadoop101上，拷贝配置
sudo scp -r /etc/profile root@hadoop102:/etc/profile
sudo scp -r /etc/profile root@hadoop103:/etc/profile

# 在hadoop102、hadoop103上，配置生效
source /etc/profile

# 测试安装是否成功
java -version
hadoop version
```

#### 2.3、远程同步工具rsync：

```shell
rsync主要用于备份和镜像。具有速度快、避免 复制相同内容和支持符号链接的有点。
rsync和scp区别：用rsync做文件复制要比scp的速度快，rsync只对差异文件做更新。scp是把所有文件都复制过去。

1、基本语法
rsync		-rvl		$pdir/$fname			$user@hadoop$host:$pdir/$fname
命令        选项参数    要拷贝的文件路径/名称        目标用户@主机：目标路径/名称
-r 递归
-v 显示复制过程
-l 拷贝符号连接

# 把hadoop101机器上的/opt/software目录同步到hadoop102服务器的root用户下的/opt/目录
rsync -rvl /opt/software/ root@hadoop102:/opt/software
```

实例操作：

```shell
# 创建脚本文件
cd ~
mkdir bin
vim bin/xsync
```

脚本文件内容：

```shell
#!/bin/bash
#1 获取输入参数个数，如果没有参数，直接退出
pcount=$#
if((pcount==0)); then
echo no args;
exit;
fi

#2 获取文件名称
p1=$1
fname=`basename $p1`
echo fname=$fname

#3 获取上级目录到绝对路径
pdir=`cd -P $(dirname $p1); pwd`
echo pdir=$pdir

#4 获取当前用户名称
user=`whoami`

#5 循环
for((host=102; host<=103; host++)); do
        echo ------------------- hadoop$host --------------
        rsync -rvl $pdir/$fname $user@hadoop$host:$pdir
done
```

```shell
# 给脚本赋最高权限
cd ~/bin
chmod 777 xsync

# 调用脚本
xsync /opt/module/hadoop-2.7.2/

# 测试是否成功
cat /opt/module/hadoop-2.7.2/etc/hadoop/core-site.xml
```

注意：

```shell
# 查看PATH是否含有用户目录
echo $PATH
```

### 3、集群配置

#### 3.1、集群部署规划：


|      | hadoop101            | hadoop102                      | hadoop103                     |
| ---- | -------------------- | ------------------------------ | ----------------------------- |
| HDFS | NameNode<br>DataNode | <br>DataNode                   | SecondaryNameNode<br>DataNode |
| YARN | <br>NodeManager      | ResourceManager<br>NodeManager | <br>NodeManager               |

#### 3.2、配置集群：

##### 3.2.1、核心配置文件

```shell
# 配置core-site.xml
cd /opt/module/hadoop-2.7.2/etc/hadoop
vi core-site.xml
```

修改文件中如下内容：

```xml
<!-- 指定HDFS中NameNode的地址 -->
<property>
		<name>fs.defaultFS</name>
      <value>hdfs://hadoop101:9000</value>
</property>

<!-- 指定Hadoop运行时产生文件的存储目录 -->
<property>
		<name>hadoop.tmp.dir</name>
		<value>/opt/module/hadoop-2.7.2/data/tmp</value>
</property>
```

##### 3.2.2、HDFS配置文件

```shell
# 配置hadoop-env.sh
cd /opt/module/hadoop-2.7.2/etc/hadoop
vi hadoop-env.sh

# 修改内容
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

```shell
# 配置hdfs-site.xml
cd /opt/module/hadoop-2.7.2/etc/hadoop
vi hdfs-site.xml
```

修改文件中如下内容：

```xml
<property>
		<name>dfs.replication</name>
		<value>3</value>
</property>

<!-- 指定Hadoop辅助名称节点主机配置 -->
<property>
      <name>dfs.namenode.secondary.http-address</name>
      <value>hadoop103:50090</value>
</property>
```

##### 3.2.3、YARN配置文件

```shell
# 配置yarn-env.sh
cd /opt/module/hadoop-2.7.2/etc/hadoop
vi yarn-env.sh

# 修改内容
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

```shell
# 配置yarn-site.xml
cd /opt/module/hadoop-2.7.2/etc/hadoop
vi yarn-site.xml
```

修改文件中如下内容：

```xml
<!-- Reducer获取数据的方式 -->
<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
</property>

<!-- 指定YARN的ResourceManager的地址 -->
<property>
		<name>yarn.resourcemanager.hostname</name>
		<value>hadoop102</value>
</property>
```

##### 3.2.4、MapReduce配置文件

```shell
# 配置mapred-env.sh
cd /opt/module/hadoop-2.7.2/etc/hadoop
vi mapred-env.sh

# 修改内容
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

```shell
# 配置mapred-site.xml
cd /opt/module/hadoop-2.7.2/etc/hadoop
cp mapred-site.xml.template mapred-site.xml
vi mapred-site.xml
```

修改文件中如下内容：

```xml
<!-- 指定MR运行在Yarn上 -->
<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
</property>
```

#### 3.3、在集群上分发配置好的Hadoop配置文件

```shell
# 调用脚本
xsync /opt/module/hadoop-2.7.2/
# 注意其他两台服务器的文件夹权限

# 测试
cat /opt/module/hadoop-2.7.2/etc/hadoop/core-site.xml
```

#### 3.4、SSH无密登录配置

```shell
# 生成公钥和私钥：
[jay@hadoop101 .ssh]$ ssh-keygen -t rsa
# 然后敲（三个回车），就会生成两个文件id_rsa（私钥）、id_rsa.pub（公钥）

# 将公钥拷贝到要免密登录的目标机器上
[jay@hadoop101 .ssh]$ ssh-copy-id hadoop101
[jay@hadoop101 .ssh]$ ssh-copy-id hadoop102
[jay@hadoop101 .ssh]$ ssh-copy-id hadoop103

# 注意：
# 还需要在hadoop102、hadoop103上采用jay账号生成公钥和私钥
# 还需要在hadoop102、hadoop103上采用jay账号配置一下无密登录到hadoop102、hadoop103、hadoop104服务器上。

# 还需要在hadoop102上采用root账号，配置一下无密登录到hadoop102、hadoop103、hadoop104；
[jay@hadoop102 opt]$ su root
[root@hadoop102 opt]# cd 
[root@hadoop102 ~]# ssh-keygen -t rsa
[root@hadoop102 .ssh]# ssh-copy-id hadoop101
[root@hadoop102 .ssh]# ssh-copy-id hadoop102
[root@hadoop102 .ssh]# ssh-copy-id hadoop103
```

#### 3.5、群起集群

##### 3.5.1、配置slaves

```shell
[jay@hadoop101 ~]$ cd /opt/module/hadoop-2.7.2/etc/hadoop
[jay@hadoop101 hadoop]$ vi slaves

# 在文件中增减加如下内容：
hadoop101
hadoop102
hadoop103
#注意：该文件中添加的内容结尾不允许有空格，文件中不允许有空行。

#同步所有节点配置文件
[jay@hadoop101 hadoop]$ xsync slaves
```

##### 3.5.2、启动HDFS

```shell
# 查看进程，如果正在运行需要停止
jps

# 如果是第一次启动，需要格式化NameNode
[jay@hadoop101 hadoop-2.7.2]$ bin/hdfs namenode -format

# 启动HDFS
[jay@hadoop101 hadoop-2.7.2]$ sbin/start-dfs.sh
Starting namenodes on [hadoop101]
hadoop101: starting namenode, logging to /opt/module/hadoop-2.7.2/logs/hadoop-jay-namenode-hadoop101.out
hadoop103: starting datanode, logging to /opt/module/hadoop-2.7.2/logs/hadoop-jay-datanode-hadoop103.out
hadoop102: starting datanode, logging to /opt/module/hadoop-2.7.2/logs/hadoop-jay-datanode-hadoop102.out
hadoop101: starting datanode, logging to /opt/module/hadoop-2.7.2/logs/hadoop-jay-datanode-hadoop101.out
Starting secondary namenodes [hadoop103]
hadoop103: starting secondarynamenode, logging to /opt/module/hadoop-2.7.2/logs/hadoop-jay-secondarynamenode-hadoop103.out

[jay@hadoop101 hadoop-2.7.2]$ jps
2354 Jps
2133 DataNode
1999 NameNode

[jay@hadoop102 ~]$ jps
1952 Jps
1868 DataNode

[jay@hadoop103 ~]$ jps
1971 SecondaryNameNode
2026 Jps
1867 DataNode
```

##### 3.5.3、启动YARN

```shell
[jay@hadoop102 ~]$ sbin/start-yarn.sh
# 注意：NameNode和ResourceManger如果不是同一台机器，不能在NameNode上启动 YARN，应该在ResouceManager所在的机器上启动YARN。

[jay@hadoop102 hadoop-2.7.2]$ jps
2338 Jps
2069 ResourceManager
1868 DataNode
2174 NodeManager

[jay@hadoop101 hadoop-2.7.2]$ jps
2531 Jps
2436 NodeManager
2133 DataNode
1999 NameNode

[jay@hadoop103 ~]$ jps
2114 NodeManager
2210 Jps
1971 SecondaryNameNode
1867 DataNode

```

#### 3.6、Web端查看集群

```shell
# web端查看HDFS文件系统
http://hadoop101:50070

# web端查看SecondaryNameNode
http://hadoop103:50090/status.html
```

#### 3.7、集群基本测试

```shell
# 上传文件到集群
# 创建文件夹
hdfs dfs -mkdir -p /user/jay/input

# 上传文件
hdfs dfs -put /opt/software/hadoop-2.7.2.tar.gz /user/jay/input

# 查找源文件
[jay@hadoop101 subdir0]$ pwd
/opt/module/hadoop-2.7.2/data/tmp/dfs/data/current/BP-725808985-192.168.92.101-1548344259952/current/finalized/subdir0/subdir0
[jay@hadoop101 subdir0]$ cat blk_1073741825 >> tmp.txt
[jay@hadoop101 subdir0]$ cat blk_1073741826 >> tmp.txt
[jay@hadoop101 subdir0]$ tar -zxvf tmp.txt

# 命令下载文件
bin/hadoop fs -get /user/jay/input/hadoop-2.7.2.tar.gz ./
```

#### 3.8、集群启动/停止方式总结

```shell
1.	各个服务组件逐一启动/停止
	（1）分别启动/停止HDFS组件
		hadoop-daemon.sh  start / stop  namenode / datanode / secondarynamenode
	（2）启动/停止YARN
		yarn-daemon.sh  start / stop  resourcemanager / nodemanager
2.	各个模块分开启动/停止（配置ssh是前提）常用
	（1）整体启动/停止HDFS
		start-dfs.sh   /  stop-dfs.sh
	（2）整体启动/停止YARN
		start-yarn.sh  /  stop-yarn.sh
```

#### 3.9、集群时间同步xxxxxxx

## 第五章 Hadoop编译源码（面试重点）

### 5.1、前期准备工作

配置CentOS能连接外网。Linux虚拟机ping [www.baidu.com](http://www.baidu.com) 是畅通的

注意：采用root角色编译，减少文件夹权限出现问题

jar包准备：

1. hadoop-2.7.2-src.tar.gz
2. jdk-8u144-linux-x64.tar.gz
3. apache-ant-1.9.9-bin.tar.gz（build工具，打包用的）
4. apache-maven-3.0.5-bin.tar.gz
5. protobuf-2.5.0.tar.gz（序列化的框架）

### 5.2、jar包安装

注意：所有操作必须在root用户下完成

1、JDK解压、配置环境变量 JAVA_HOME和PATH，验证java-version(如下都需要验证是否配置成功)

```shell
[root@hadoop-encoding software]# tar -zxvf jdk-8u144-linux-x64.tar.gz -C /opt/module/

[root@hadoop-encoding software]# vi /etc/profile
#JAVA_HOME：
export JAVA_HOME=/opt/module/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin

[root@hadoop-encoding software]# source /etc/profile

验证命令：java -version
```

2、Maven解压、配置  MAVEN_HOME和PATH

```shell
[root@hadoop-encoding software]# tar -zxvf apache-maven-3.0.5-bin.tar.gz -C /opt/module/

[root@hadoop-encoding apache-maven-3.0.5]# vi conf/settings.xml
# 增加阿里仓库
        <mirror>
                <id>nexus-aliyun</id>
                <mirrorOf>central</mirrorOf>
                <name>Nexus aliyun</name>
                <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>

[root@hadoop-encoding software]# vi /etc/profile
#MAVEN_HOME
export MAVEN_HOME=/opt/module/apache-maven-3.0.5
export PATH=$PATH:$MAVEN_HOME/bin

[root@hadoop-encoding software]# source /etc/profile

验证命令：mvn -version
```

3、ant解压、配置  ANT _HOME和PATH

```shell
[root@hadoop-encoding software]# tar -zxvf apache-ant-1.9.9-bin.tar.gz -C /opt/module/

[root@hadoop-encoding software]# vi /etc/profile
#ANT_HOME
export ANT_HOME=/opt/module/apache-ant-1.9.9
export PATH=$PATH:$ANT_HOME/bin

[root@hadoop-encoding software]# source /etc/profile

验证命令：ant -version
```

4、安装  glibc-headers 和  g++

```shell
[root@hadoop-encoding software]# yum install glibc-headers
[root@hadoop-encoding software]# yum install gcc-c++
```

5、安装make和cmake

```shell
[root@hadoop-encoding software]# yum install make
[root@hadoop-encoding software]# yum install cmake

```
6、安装protobuf 

```shell
# 解压protobuf
[root@hadoop-encoding software]# tar -zxvf protobuf-2.5.0.tar.gz -C /opt/module/

# 进入到解压后protobuf主目录
cd /opt/module/protobuf-2.5.0

# 执行命令
[root@hadoop-encoding protobuf-2.5.0]# ./configure
[root@hadoop-encoding protobuf-2.5.0]# make
[root@hadoop-encoding protobuf-2.5.0]# make check
[root@hadoop-encoding protobuf-2.5.0]# make install
[root@hadoop-encoding protobuf-2.5.0]# ldconfig

[root@hadoop-encoding protobuf-2.5.0]# vi /etc/profile
#LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/opt/module/protobuf-2.5.0
export PATH=$PATH:$LD_LIBRARY_PATH

[root@hadoop-encoding protobuf-2.5.0]# source /etc/profile
验证命令：protoc --version
```
7、安装openssl库

```shell
[root@hadoop-encoding software]# yum install openssl-devel
```
8、安装 ncurses-devel库

```shell
[root@hadoop-encoding software]# yum install ncurses-devel
```
到此，编译工具安装基本完成。

### 5.3、编译源码

1、解压源码到/opt/目录

```shell
[root@hadoop-encoding software]# tar -zxvf hadoop-2.7.2-src.tar.gz -C /opt/
```

2、进入到hadoop源码主目录

```shell
[root@hadoop-encoding hadoop-2.7.2-src]# pwd
/opt/hadoop-2.7.2-src
```

3、通过maven执行编译命令

```shell
[root@hadoop-encoding hadoop-2.7.2-src]# mvn package -Pdist,native -DskipTests -Dtar
```
等待时间30分钟左右，最终成功是全部SUCCESS

# 大数据技术之Hadoop（HDFS）

## 第一章 HDFS概述

### 1.1、HDFS产生背景及定义

产生背景：

​	随着数据量越来越大，在一个操作系统存不下所有的数据，那么久分配到更多的操作系统管理的磁盘中，但是不方便管理和维护，迫切需要一种系统来管理多台机器上的文件，这就是分布式文件管理系统。HDFS只是分布式文件管理系统的一种。

HDFS定义：

​	HDFS（Hadoop Distributed File System），它是一个文件系统，用于存储文件，通过目录树来定位文件；其次，它是分布式的，由很多服务器联合起来实现其功能，集群中的服务器有各自的角色。

​	HDFS的使用场景：适合一次写入，多次读书的场景，且不支持文件的修改。适合用来做数据分析，并不适合用来做网盘应用。

### 1.2、HDFS优缺点

优点：

1）高容错性：

​	1.1）数据自动保存过个副本，它通过增加副本的形式，提高容错性。

​	1.2）某个副本丢失后，它可以自动恢复。

![HDFS优点高容错性](大数据md图片\HDFS优点高容错性.png)

2）适合处理大数据：

​	2.1）数据规模：能够处理数据规模达到GB、TB、甚至PB级别的数据。

​	2.2）文件规模：能够处理百万规模以上的文件数量，数量相当之大。

3）可构建在廉价机器上，通过多副本机制，提高可靠性。

缺点：

1）不适合低延迟时数据访问，比如毫秒级的存储数据，是做不到的。

2）无法高效的对大量小文件进行存储。

​	2.1）存储大量小文件的话，会占用NameNode大量的内存来存储文件目录和块信息。这样是不可取的，因为NameNode的内存总是有限的。

​	2.2）小文件存储的寻址时间会超过读取时间，它违反了HDFS的设计目标。

3）不支持并发写入、文件随机修改。

​	3.1）一个文件只能有一个写，不允许多个线程同时写。

​	3.2）仅支持数据append(追加)，不支持文件的随机修改。

### 1.3、HDFS组成架构

```shell
1、NameNode(nn)：就是Master，它是一个主管、管理者。
	1）管理HDFS的名称空间
	2）配置副本策略
	3）管理数据块（Block）映射信息
	4）处理客户端读写请求
2、DataNode：就是Slave。NameNode下达命令，DataNode执行实际操作。
	1）存储实际的数据块
	2）执行数据块的读/写操作
3、Client：就是客户端
	1）文件切分。文件上传HDFS的时候，Client将文件分成一个一个的Block，然后进行上传
	2）与NameNode交互，获取文件的位置信息
	3）与DataNode交互，读取或写入数据
	4）Client提供一些命令来管理HDFS，比如NameNode格式化
	5）Client可以通过一些命令来访问HDFS，比如对HDFS增删查改操作
4、SecondaryNameNode：并非NameNode的热备。当NameNode挂掉的时候，它并不能马上替换NameNode并提供服务。
	1）辅助NameNode，分担其工作量。比如定期合并Fsimage和Edits，并推送给NameNode
	2）在紧急情况下，可辅助恢复NameNode
```

### 1.4、HDFS文件块大小（面试重点）

- HDFS中的文件在物理上是分块存储（Block），块的大小可以通过配置参数（dfs.blocksize）来规定。
- 默认大小在Hadoop2.x版本中是128M，老版本中是64M。
- 寻址时间为传输时间的1%时，则为最佳状态。

```shell
思考：为什么块的大小不能设置太小，也不能设置太大。
1、HDFS的块设置太小，会增加寻址时间，程序一直在找块的开始位置。
2、如果块设置的太大，从磁盘传输数据的时间会明显大于定位这个块开始位置所需的时间。导致程序在处理这块数据时，会非常慢。
总结：HDFS块的大小设置主要取决于磁盘传输速率。
```

## 第二章 HDFS的Shell操作（开发重点）

### 2.1、基本语法

```shell
bin/hadoop fs 【命令】
bin/hadoop dfs 【命令】
# dfs是fs的实现类
```

### 2.2、命令大全

```shell
bin/hadoop fs
bin/hadoop dfs
```

### 2.3、常用命令实操

```shell
# 0）启动Hadoop集群
sbin/start-dfs.sh
sbin/start-yarn.sh

# 1）-help 输出这个命令参数
语法：hadoop fs -help 【命令】
hadoop fs -help rm

# 2）-ls 显示目录信息
hadoop fs -ls /

# 3）-mkdir 在HDFS上创建目录
hadoop fs -mkdir -p /sanguo/shuguo
加 -p 在创建文件夹连同父类一起创建，不加-p 在创建文件夹会发生找不到父文件夹的情况

# 4）-moveFromLocal 从本地剪切粘贴到HDFS
语法
hadoop fs -moveFromLocal 【本地路径】【HDFS路径】
相对路径
hadoop fs -moveFromLocal ./1.txt /
hadoop fs -moveFromLocal 2.txt /
绝对路径
hadoop fs -moveFromLocal /opt/xxx/2.txt /

# 5）-appendToFile 把本地文件的内容，追加HDFS的某个文件末尾
语法
hadoop fs -appendToFile 【本地路径】【HDFS路径】
hadoop fs -appendToFile 3.txt /1.txt

# 6）-cat 显示文件内容
hadoop fs -cat 【HDFS路径】/【文件名】

# 7）-chgrp、-chmod、-chown 修改文件所属权限
hadoop fs -chmod 666 /1.txt
hadoop fs -chmod 777 /2.txt

# 8）-copyFromLocal 从本地拷贝到HDFS
hadoop fs -copyFromLocal 3.txt /

# 9）-copyToLocal 从HDFS拷贝到本地
语法
hadoop fs -copyToLocal 【HDFS路径】/【文件名】 【本地路径】
相对路径
hadoop fs -copyToLocal /1.txt ./
绝对路径
hadoop fs -copyToLocal /1.txt /opt/module/xxx

# 10）-cp 在HDFS里复制一个文件到HDFS的另一个路径
语法
hadoop fs -cp 【HDFS路径 src】【HDFS路径 dest】
hadoop fs -cp /1.txt /4.txt

# 11）-mv 在HDFS里移动一个文件
语法
hadoop fs -mv 【HDFS路径 src】【HDFS路径 dest】
hadoop fs -mv /1.txt /5.txt

# 12）-get 等同于copyToLocal，从HDFS下载到本地

# 13）-getmerge 合并下载多个文件
语法
hadoop fs -getmerge 【HDFS路径】【本地路径】
下载
hadoop fs -getmerge /weiguo1 ./zaiyiqi.txt
hadoop fs -getmerge /weiguo1/* ./zaiyiqi.txt
hadoop fs -getmerge /weiguo1/*.txt ./zaiyiqi3.txt

# 14）-put 等同于copyFromLocal，从本地拷贝到HDFS

# 15）-tail 显示一个文件末尾
hadoop fs -tail /LICENSE.txt

# 16）-rm 删除文件或文件夹
删除文件
hadoop fs -rm /5.txt
删除文件夹 -r递归
hadoop fs -rm -r /sanguo
hadoop fs -rm -R /sanguo

# 17）-rmdir 删除空文件夹
hadoop fs -rmdir /weiguo2

# 18）-du 统计文件夹大小的信息
[jay@hadoop101 hadoop-2.7.2]$ hadoop fs -du /
15429      /LICENSE.txt
101        /NOTICE.txt
197657687  /hadoop-2.7.2.tar.gz

[jay@hadoop101 hadoop-2.7.2]$ hadoop fs -du -h /
15.1 K   /LICENSE.txt
101      /NOTICE.txt
188.5 M  /hadoop-2.7.2.tar.gz

[jay@hadoop101 hadoop-2.7.2]$ hadoop fs -du -s -h /
188.5 M  /

# 19）-setrep 设置HDFS中文件的副本数量
hadoop fs -setrep 10 /sanguo/shuguo/kongming.txt
注：这里设置的副本数只是记录在NameNode的元数据中，是否真的有那么多副本还得看DataNode的数量。因为目前只有3台设备，最多也就3个副本，只有节点数增加到10台时，副本数才能达到10。
```

## 第三章 HDFS客户端操作（开发重点）

### 3.1、HDFS客户端环境准备

#### 3.1.1、windows安装环境

```shell
# 配置JAVA_HOME，注意不能带有空格
	D:\Develop\Java\jdk1.8.0_181
# 配置PATH
	%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
# 查看版本
	java -version

# 配置HADOOP_HOME
	D:\Develop\hadoop-2.7.2
# 配置PATH
	%HADOOP_HOME%\bin;
# 查看版本
	hadoop version
```

#### 3.1.2、Maven工程配置

配置 pom.xml

```xml
<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>2.8.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-common</artifactId>
			<version>2.7.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-client</artifactId>
			<version>2.7.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-hdfs</artifactId>
			<version>2.7.2</version>
		</dependency>
		<dependency>
			<groupId>jdk.tools</groupId>
			<artifactId>jdk.tools</artifactId>
			<version>1.8</version>
			<scope>system</scope>
			<systemPath>${JAVA_HOME}/lib/tools.jar</systemPath>
		</dependency>
</dependencies>
```

配置 log4j.properties

```properties
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=target/spring.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```

#### 3.1.3、创建HdfsClient类

```java
import java.net.URI;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.junit.Test;

public class HdfsClient {

	public static void main(String[] args) throws Exception {
		// 1 获取hdfs客户端对象
		Configuration conf = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), conf, "jay");
		
		// 2 在HDFS上创建路径
		fs.mkdirs(new Path("/0529/dashen/2.txt"));
		
		// 3 关闭资源
		fs.close();
		
		System.out.println("over");
	}
}
```

### 3.2、HDFS的API操作

#### 3.2.1、HDFS文件上传（测试参数优先级）

```java
	@Test
	public void testCopyFromLocalFile() throws Exception {
		// 1 获取hdfs客户端对象
		Configuration conf = new Configuration();
		conf.set("dfs.replication", "2");
		
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), conf, "jay");
		
		// 2 上传文件
		fs.copyFromLocalFile(new Path("e:/DateTest2.java"), new Path("/"));
		
		// 3 关闭资源
		fs.close();
		
		System.out.println("over");
	}
```

文件 hdfs-site.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
	<property>
		<name>dfs.replication</name>
        <value>1</value>
	</property>
</configuration>
```

参数优先级顺序：

（1）客户端代码中设置的值 > （2）ClassPatlass下用户自定义配置文件 > （3）Hadoop服务器的默认配置

#### 3.2.2、HDFS文件下载

```java
	@Test
	public void testCopyToLocalFile() throws Exception {
		
		// 1 获得HDFS客户端对象
		Configuration conf = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), conf, "jay");
		
		// 2 下载文件
		boolean delSrc = false; 	// 是否从远程删除
		Path src = new Path("/DateTest.java");	// Hadoop远程路径
		Path dst = new Path("e:/2.java");		// 本地路径
		boolean useRawLocalFileSystem = true;	// 是否开启文件校验
		fs.copyToLocalFile(delSrc, src, dst, useRawLocalFileSystem);
		
		// 3 关闭资源
		fs.close();
		System.out.println("over");
	}
```

#### 3.2.3、HDFS文件删除

```java
	@Test
	public void testDelete() throws Exception {
		
		// 1 获得HDFS客户端对象
		Configuration conf = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), conf, "jay");
		
		// 2 删除文件
		fs.delete(new Path("/0529"), true);	// 第二个参数是递归删除
		
		// 3 关闭资源
		fs.close();
		System.out.println("over");
	}
```

#### 3.2.4、HDFS文件名更改

```java
	@Test
	public void testRename() throws Exception {
		
		// 1 获得HDFS客户端对象
		Configuration conf = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), conf, "jay");
		
		// 2 修改文件名
		fs.rename(new Path("/0530"), new Path("/a0530"));
		
		// 3 关闭资源
		fs.close();
		System.out.println("over");
	}
```

#### 3.2.5、HDFS文件详情查看

```java
	@Test
	public void testListFiles() throws Exception {
		
		// 1 获得HDFS客户端对象
		Configuration conf = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), conf, "jay");
		
		// 2 获取文件列表
		RemoteIterator<LocatedFileStatus> listFiles = fs.listFiles(new Path("/user"), true);
		while(listFiles.hasNext()) {
			LocatedFileStatus next = listFiles.next();
			
			System.out.println("文件名：" + next.getPath().getName());
			System.out.println("长度：" + next.getLen());
			System.out.println("权限：" + next.getPermission());
			System.out.println("分组：" + next.getGroup());
			
			// 存储块的信息
			BlockLocation[] blockLocations = next.getBlockLocations();
			for (BlockLocation blockLocation : blockLocations) {
				String[] hosts = blockLocation.getHosts();
				for (String string : hosts) {
					System.out.println("存储块的信息：" + string);
				}
			}
		}
		
		// 3 关闭资源
		fs.close();
		System.out.println("over");
	}
```

#### 3.2.6、HDFS文件和文件夹判断

```java
	@Test
	public void testListStatus() throws Exception {
		// 1 获取文件配置信息
		Configuration configuration = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), configuration, "jay");
			
		// 2 判断是文件还是文件夹
		FileStatus[] listStatus = fs.listStatus(new Path("/"));
			
		for (FileStatus fileStatus : listStatus) {
			// 如果是文件
			if (fileStatus.isFile()) {
					System.out.println("文件："+fileStatus.getPath().getName());
				}else {
					System.out.println("文件夹:"+fileStatus.getPath().getName());
				}
			}
			
		// 3 关闭资源
		fs.close();
	}
```

### 3.3、HDFS的I/O流操作

#### 3.3.1、HDFS文件上传

```java
	@Test
	public void putFileToHDFS() throws Exception {
		// 1 获取文件配置信息
		Configuration configuration = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), configuration, "jay");
			
		// 创建本地输入流
		FileInputStream fis = new FileInputStream(new File("e:/日历.xlsx"));
		
		// 获取远程输出流
		FSDataOutputStream fos = fs.create(new Path("/日历.xlsx"));
		
		// 流对拷
		IOUtils.copyBytes(fis, fos, configuration);
		
		// 3 关闭资源
		IOUtils.closeStream(fos);
		IOUtils.closeStream(fis);
		fs.close();
	}
```

#### 3.3.2、HDFS文件下载

```java
	@Test
	public void getFileFromHDFS() throws Exception {
		// 1 获取文件配置信息
		Configuration configuration = new Configuration();
		FileSystem fs = FileSystem.get(new URI("hdfs://hadoop101:9000"), configuration, "jay");
			
		// 获取远程输入流
		FSDataInputStream fis = fs.open(new Path("/日历.xlsx"));
		
		// 创建本地输出流
		FileOutputStream fos = new FileOutputStream(new File("e:/11.xlsx"));
		
		// 流对拷
		IOUtils.copyBytes(fis, fos, configuration);
		
		// 3 关闭资源
		IOUtils.closeStream(fos);
		IOUtils.closeStream(fis);
		fs.close();
	}
```

#### 3.3.3、定位文件读取

需求：分块读取HDFS上的大文件，比如根目录下的/hadoop-2.7.2.tar.gz

1、下载第一块

```

```

2、下载第二块

```

```

3、文件合并

## 第四章 HDFS的数据流（面试重点）

### 4.1、HDFS的写数据流程

#### 4.1.1、剖析文件写入

![HDFS的写数据流程](大数据md图片\HDFS的写数据流程.png)

1. 客户端通过Distributed FileSystem模块向NameNode请求上传文件，NameNode检查目标文件是否存在，父目录是否存在。
2. NameNode返回是否可以上传。
3. 客户端请求一个Block上传到哪几个DataNode服务器上。
4. NameNode返回3个DataNode节点，分别为dn1、dn2、dn3。
5. 客户端通过FSDataOutputStream模块请求dn1上传数据，dn1收到请求会继续调用dn2，然后dn2调用dn3，将这个通信管道建立完成。
6. dn1、dn2、dn3逐级应答客户端。
7. 客户端开始往dn1上传第一个Block（先从地盘读取数据放到一个本地内存缓存），以Packet为单位，dn1收到第一个Packet就会传给dn2，dn2传给dn3；dn1每传一个Packet会放入一个应答队列等待应答。
8. 当一个Block传输完成之后，客户端再次请求NameNode上传第二个Block的服务器。（重复执行3-7步）

#### 4.1.2、网络拓扑-节点距离计算

在HDFS写数据的过程中，NameNode会选择距离待上传数据最近距离的DataNode接收数据。那么这个最近距离怎么计算呢？

​	节点距离：两个节点到达最近的共同祖先的距离总和。

#### 4.1.3、机架感知

```shell
# 官方地址
http://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Data_Replication

# 官方说明
For the common case, when the replication factor is three, HDFS’s placement policy is to put one replica on one node in the local rack, another on a different node in the local rack, and the last on a different node in a different rack.
```

#### 4.1.4、副本节点选择

第一个副本在Client所处的节点上。如果客户端在集群外，随机选一个。

第二个副本和第一个副本位于相同机架，随机节点。

第三个副本位于不同机架，随机节点。

![副本节点选择](大数据md图片\副本节点选择.png)

### 4.2、HDFS的读数据流程（面试重点）

![HDFS读数据流程](大数据md图片\HDFS读数据流程.png)

1. 客户端通过Distributed FileSystem向NameNode请求下载文件，NameNode通过查询元数据，找到文件块所在的DataNode地址。

2. 挑选一台DataNode（就近原则，然后随机）服务器，请求读取数据。

3. DataNode开始传输数据给客户端（从磁盘里面读取输入流，以Packet为单位来做校验）。

4. 客户端以Packet为单位接收，先在本地缓存，然后写入目标文件。


## 第五章 NameNode和SecondaryNameNode（面试重点）

### 5.1、NN和2NN工作机制xxxxxxxx

思考：NameNode的元数据存储在哪里？

​	首先，我们做个假设，如果存储在NameNode节点的磁盘中，因为经常需要进行随机访问，还有响应客户端请求，必然是效率过度。因此，元数据需要存放在内存中。但如果只存在内存中，一旦断电，元数据丢失，整个集群就无法工作了。因此产生在磁盘中备份原数据的FsImage。

​	这样又会带来新的问题，当在内存中的元数据更新时，如果同时更新FsImage，就会导致效率过低，但如果不更新FsImage，就会发生一致性问题，一旦NameNode节点断电，就会产生数据丢失。因此引入Edits文件（只进行追加操作，效率很高）。每当元数据有更新或者添加元数据时，修改内存中的元数据并追加到Edits中。这样，一旦NameNode节点断电，可以通过FsImage和Edits的合并，合成元数据。

​	但是，如果长时间添加数据到Edits中，会导致文件数据过大，效率降低，而且一旦断电，恢复原数据需要的时间过长。因此，需要定期进行FsImage和Edits的合并，如果这个操作由NameNode节点完成，又会效率过低。因此引入一个新的节点SecondaryNamenode，专门用于FsImage和Edits的合并。

### 5.2、Fsimage和Edits解析

```

```

### 5.3、CheckPoint时间设置

### 5.4、NameNode故障处理

### 5.5、集群安装模式

### 5.6、NameNode多目录配置



## 第六章 DataNode（面试重点）





## 第七章 HDFS2.X新特性





## 第八章 HDFS HA高可用









# 服役新数据节点



# 退役旧数据节点



# 添加白名单







# 大数据技术之Hadoop（MapReduce）

## 第一章 MapReduce概述

### 1、MapReduce定义

​	MapReduce是一个分布式运算程序的编程框架，是用户开发“基于Hadoop的数据分析应用”的核心框架。

​	MapReduce核心功能是将用户边写的业务逻辑代码和自带默认组件整合成一个完整的分布式运算程序，并发运行在一个Hadoop集群上。

### 2、MapReduce优缺点

#### 2.1、优点

1、MapReduce易于编程

​	它简单的实现一些接口，就可以完成一个分布式程序，这个分布式程序可以分不到大量廉价的PC机器上运行。也就是说你写的一个分布式程序，跟写一个简单的串行程序是一模一样的。就是因为这个特点使得MapReduce编程变得非常流行。

2、良好的扩展性

​	当你的计算资源不能得到满足的时候，你可以通过简单的增加机器来扩展它的计算能力。

3、高容错性

​	MapReduce设计的初衷就是使程序能够部署在廉价的PC机器上，这就要求它具有很高的容错性。比如其中一台机器挂了，它可以把上面的计算任务转移到另外一个节点上运行，不至于这个任务运行失败，而且这个过程中不需要人工参与，而完全是由Hadoop内部完成的。

4、适合PB级以上海量数据的离线处理

​	可以实现上千台服务器集群并发工作，提供数据处理能力。

#### 2.2、缺点

1、不擅长实时计算

​	MapReduce无法像MySQL一样，在毫秒或者秒级内返回结果。

2、不擅长流式计算

​	流式计算的输入数据是动态的，而MapReduce的输入数据集是静态的，不能动态变化。这是因为MapReduce自身的设计特点决定了数据源必须是静态的。

3、不擅长DAG（有向图）计算

​	多个应用程序存在依赖关系，吼一个应用程序的输入为前一个的输出。在这种情况下，MapReduce并不是不能做，而是使用后，每个MapReduce作业的输出结果都会写入到磁盘，会造成大量磁盘IO，导致性能的低下。

### 3、MapReduce核心思想

![MapReduce编程核心思想](大数据md图片\MapReduce编程核心思想.png)

1、分布式的运算程序往往需要分成至少2个阶段。

2、第一个阶段的MapTask并发实例，完全并行运行，互不相干。

3、第二阶段的ReduceTask并发实例互不相干，但是他们的数据依赖于上一个阶段的所有MapTask并发实例的输出。

4、MapReduce编程模型只能包含一个Map阶段和一个Reduce阶段，如果用户的业务逻辑非常复杂，那就只能多个MapReduce程序串行运行。

总结：分析WordCount数据流走向深入理解MapReduce核心思想。

### 4、MapReduce进程

一个完整的MapReduce程序在分布式运行时有三类实例进程：

1、MrAppMaster：负责整个程序的过程调度及状态协调。

2、MapTask：负责Map阶段的整个数据处理流程。

3、ReduceTask：负责Reduce阶段的整个数据处理流程。

### 5、官方WordCount源码

​	采用反编译工具编译源码，发现WordCount案例有Map类、Reduce类和驱动类。且数据的类型是Hadoop自身封装的序列化类型。

### 6、常用数据序列化类型

常用的数据类型对应Hadoop数据序列化类型：

1、Java类型：boolean、byte、int、float、long、double、map、array

​	Hadoop Writable类型：首字母大写在后面加Writable， 例如boolean --> BooleanWritable

2、字符串类型：String 对应 Text

### 7、MapReduce编程规范

#### 7.1、Mapper阶段

1、用户自定义的Mapper要继承自己的父类

2、Mapper的输入数据是KV键值对的形式（KV的类型可自定义）

3、Mapper中的业务逻辑写在map()方法中

4、Mapper的输出数据是KV键值对的形式（KV的类型可自定义）

5、map()方法（MapTask进程）对每一个<K,V>调用一次

#### 7.2、Reduce阶段

1、用户自定义的Reduce要继承自己的父类

2、Reduce的输入数据类型对应Mapper的输出数据类型，也是KV

3、Reduce的业务逻辑写在reduce()方法中

4、ReduceTask进程对每一组相同k的<K,V>组调用一次reduce()方法

#### 7.3、Driver阶段

相当于YARN集群的客户端，用于提交我们整个程序到YARN集群，提交的是封装了MapReduce程序相关运行参数的job对象。

### 8、WordCount案例实操

#### 8.1、需求

在给定的文本文件中统计处每一个单词出现的总词数

#### 8.2、环境准备

1、创建maven工程

2、配置pom.xml

```xml
<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>2.8.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-common</artifactId>
			<version>2.7.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-client</artifactId>
			<version>2.7.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-hdfs</artifactId>
			<version>2.7.2</version>
		</dependency>
</dependencies>
```

3、log4j.properties

```properties
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=target/spring.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```

4、编写程序Mapper

```java
/**
 * map阶段
 * KEYIN 输入数据的key
 * VALUEIN 输入数据的value
 * KEYOUT 输出数据的类型 atguigu,1  ss,1
 * VALUEOUT 输出数据的value类型
 */
public class WordCountMapper extends Mapper<LongWritable, Text, Text, IntWritable>{

	Text k = new Text();
	IntWritable v = new IntWritable(1);
	
	@Override
	protected void map(LongWritable key, Text value, Context context)
			throws IOException, InterruptedException {
		// 数据： atguigu atguigu
		
		// 1 获取一行数据
		String line = value.toString();
		
		// 2 切个单词
		String[] words = line.split(" ");
		
		// 3 循环写出
		for (String word : words) {
			k.set(word);	// atguigu
			context.write(k, v);
		}
	}
}

```

5、编写程序Reducer

```java
/**
 * KEYIN,VALUEIN	map阶段输出的key和value
 * KEYOUT,VALUEOUT  输出的key和value
 */
public class WordCountReducer extends Reducer<Text, IntWritable, Text, IntWritable>{

	IntWritable v = new IntWritable();
	
	@Override
	protected void reduce(Text key, Iterable<IntWritable> values,
			Context context) throws IOException, InterruptedException {
		// 数据1： atguigu 1
		// 数据2： atguigu 1
		
		int sum = 0;
		
		// 1 累加求和
		for (IntWritable value : values) {
			sum += value.get();
		}
		
		// 2 写出
		v.set(sum);
		context.write(key, v);
	}

}

```

6、编写程序Driver

```java
public class WordCountDriver {

	public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
		Configuration conf = new Configuration();
		// 1 获取Job对象
		Job job = Job.getInstance(conf);

		// 2 设置jar存放位置
		job.setJarByClass(WordCountDriver.class);
		
		// 3 关联Map和Reduce类
		job.setMapperClass(WordCountMapper.class);
		job.setReducerClass(WordCountReducer.class);
		
		// 4 设置Mapper阶段输出数据的key和value类型
		job.setMapOutputKeyClass(Text.class);
		job.setMapOutputValueClass(IntWritable.class);
		
		// 5 设置最终数据输出的key和value类型
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(IntWritable.class);
		
		// 6 设置程序运行的输入路径和输出路径
		FileInputFormat.setInputPaths(job, new Path(args[0]));
		FileOutputFormat.setOutputPath(job, new Path(args[1]));
		
		// 7 提交job
		// job.submit(); // 过时的方法
		boolean result = job.waitForCompletion(true);	// 打印日志 true=打印 false=不打印
		System.exit(result?0:1);
	}
}

```

8、集群上测试

用maven打jar包，需要添加的打包插件依赖

注意：mainClass部分需要替换为自己工程主类

```xml
<build>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.3.2</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
			<plugin>
				<artifactId>maven-assembly-plugin </artifactId>
				<configuration>
					<descriptorRefs>
						<descriptorRef>jar-with-dependencies</descriptorRef>
					</descriptorRefs>
					<archive>
						<manifest>
							<mainClass>com.atguigu.mr.WordcountDriver</mainClass>
						</manifest>
					</archive>
				</configuration>
				<executions>
					<execution>
						<id>make-assembly</id>
						<phase>package</phase>
						<goals>
							<goal>single</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>
```

将程序打成jar包，然后拷贝到Hadoop集群中

右键->Run as->maven install。等待编译完成就会在项目的target文件夹中生成jar包。

```shell
[jay@hadoop101 hadoop-2.7.2]$ hadoop jar [文件名].jar com.jay.WordCountDriver /input /output
```

7、报错解决：

```shell
Permission denied: user=dr.who, access=READ_EXECUTE, inode="/tmp":jay:superg

# 给tmp赋777权限
[jay@hadoop101 hadoop-2.7.2]$ hadoop fs -chmod 777 /tmp
```

## 第二章 Hadoop序列化

### 2.1、序列化概述

#### 2.1.1、什么是序列化

序列化：就是把内存中的对象，转换成字节序列（或其他数据传输协议）以便于存储到磁盘（持久化）和网络传输。

反序列化：就是将收到字节序列（或其他数据传输协议）或者是磁盘的持久化数据，转换成内存中的对象。

#### 2.1.2、为什么要序列化

一般来说，“活的”对象只存在内存里，关机断电就没有了。而且“活的”对象只能由本地的进程使用，不能被发送到网络上的另外一台计算机。然而序列化可以存储“活的”对象，可以将“活的”对象发送到远程计算机。

#### 2.1.3、为什么不用Java的序列化

Java的序列化是一个重量级序列化框架（Serializable），一个对象被序列化后，会附带很多额外的信息（各种校验信息，Header，继承体系等），不便于在网络中高效传输。所以，Hadoop自己开发了一套序列化机制（Writable）。

Hadoop序列化特点：

1. 紧凑：高效使用存储空间。
2. 快速：读写数据的额外开销小。
3. 可扩展：随着通信协议的升级而可升级。
4. 互操作：支持多语言的交互。

### 2.2、自定义bean对象实现序列化接口（Writable）

在企业开发中往往常用的基本序列化类型不能满足所有需求，比如在Hadoop框架内部传递一个bean对象，Name该对象就需要实现序列化接口。

具体实现bean对象序列化步骤如下7步。

1、必须实现Writable接口

2、反序列化时，需要反射调用空参构造函数，所以必须有空参构造

```java
public FlowBean() {
	super();
}
```

3、重写序列化方法

```java
@Override
public void write(DataOutput out) throws IOException {
	out.writeLong(upFlow);
	out.writeLong(downFlow);
	out.writeLong(sumFlow);
}
```

4、重写反序列化方法

```java
@Override
public void readFields(DataInput in) throws IOException {
	upFlow = in.readLong();
	downFlow = in.readLong();
	sumFlow = in.readLong();
}
```

5、注意反序列化的顺序和序列化的顺序完全一致

6、要想把结果显示在文件中，需要重写toString()，可用“\t”分开，方便后需用。

7、如果需要将自定义的bean放在key中传输，则还需要实现Comparable接口，因为MapReduce框中的Shuffle过程要求对key必须能排序。详见后面排序案例。

```java
@Override
public int compareTo(FlowBean o) {
	// 倒序排列，从大到小
	return this.sumFlow > o.getSumFlow() ? -1 : 1;
}
```

### 2.3、序列化实操

1、需求

​	统计每一个手机号耗费的总上行流量、下行流量、总流量

2、编写流量统计的Bean对象

```java
// 1 实现Writable接口
public class FlowBean implements Writable {

	private long upFlow;	// 上行流量
	private long downFlow;	// 下行流量
	private long sumFlow;	// 总流量
	
	// 2 空参构造，反序列化时需要反射调用空参构造函数
	public FlowBean() {
		super();
	}
	
	public FlowBean(long upFlow, long downFlow) {
		super();
		this.upFlow = upFlow;
		this.downFlow = downFlow;
		this.sumFlow = upFlow + downFlow;
	}

	// 3 序列化方法
	@Override
	public void write(DataOutput out) throws IOException {
		out.writeLong(upFlow);
		out.writeLong(downFlow);
		out.writeLong(sumFlow);
	}

	// 4 反序列化方法
	@Override
	public void readFields(DataInput in) throws IOException {
		// 5 反序列化的读顺序，必须和写序列化方法的写顺序保持一致
		upFlow = in.readLong();
		downFlow = in.readLong();
		sumFlow = in.readLong();
	}

	public long getUpFlow() {
		return upFlow;
	}

	public void setUpFlow(long upFlow) {
		this.upFlow = upFlow;
	}

	public long getDownFlow() {
		return downFlow;
	}

	public void setDownFlow(long downFlow) {
		this.downFlow = downFlow;
	}

	// 6 编写toString方法，方便打印输出到文本
	@Override
	public String toString() {
		return upFlow + "\t" + downFlow + "\t" + sumFlow;
	}

	public void set(long upFlow2, long downFlow2) {
		upFlow = upFlow2;
		downFlow = downFlow2;
		sumFlow = upFlow2 + downFlow2;
	}
}
```

3、编写Mapper类

```java
public class FlowCountMapper extends Mapper<LongWritable, Text, Text, FlowBean>{

	Text k = new Text();
	FlowBean v = new FlowBean();
	
	@Override
	protected void map(LongWritable key, Text value, Context context)
			throws IOException, InterruptedException {
		// 1	13736230513	192.196.100.1	www.atguigu.com	2481	24681	200
		// 1 获取一行
		String line = value.toString();
		
		// 2 切割 \t
		String[] fields = line.split("\t");
		
		// 3 封装对象
		k.set(fields[1]);	// 封装手机号
		long upFlow = Long.parseLong(fields[fields.length - 3]);
		long downFlow = Long.parseLong(fields[fields.length - 2]);
		
		v.setUpFlow(upFlow);
		v.setDownFlow(downFlow);
		
		// 4 写出
		context.write(k, v);
	}
}
```

4、编写Reduce类

```java
public class FlowCountReducer extends Reducer<Text, FlowBean, Text, FlowBean>{

	FlowBean v = new FlowBean();
	
	@Override
	protected void reduce(Text key, Iterable<FlowBean> values, Context context)
			throws IOException, InterruptedException {
		// 13568436656	 2481	24681 26485
		// 13568436656	 1116	954 2200
		
		long sum_upFlow = 0;
		long sum_downFlow = 0;
		
		// 1 累加求和
		for (FlowBean flowBean : values) {
			sum_upFlow += flowBean.getUpFlow();
			sum_downFlow += flowBean.getDownFlow();
		}
		
		v.set(sum_upFlow, sum_downFlow);
		
		// 2 写出
		context.write(key, v);
	}
}
```

5、编写Driver驱动类

```java
public class FlowsumDriver {
	public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
		args = new String[] {"D:\\hadoop_input_flow","D:\\hadoop_out_flow"};
		
		Configuration conf = new Configuration();
		
		// 1 获取job对象
		Job job = Job.getInstance(conf);
		
		// 2 设置jar的路径
		job.setJarByClass(FlowsumDriver.class);
		
		// 3 关联mapper和reduce
		job.setMapperClass(FlowCountMapper.class);
		job.setReducerClass(FlowCountReducer.class);
		
		// 4 设置mapper输出的key和value类型
		job.setMapOutputKeyClass(Text.class);
		job.setMapOutputValueClass(FlowBean.class);
		
		// 5 设置最终输出的key和value类型
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(FlowBean.class);
		
		// 6 设置最终输出的key和value类型
		FileInputFormat.setInputPaths(job, new Path(args[0]));
		FileOutputFormat.setOutputPath(job, new Path(args[1]));
		
		// 7 提交job
		boolean result = job.waitForCompletion(true);
		System.exit(result?0:1);
	}
}
```

## 第三章 框架原理

### 3.1、InputFormat 数据输入

#### 3.1.1、切片与MapTask并行度决定机制

1、问题引出

​	MapTask的并行度决定Map阶段的任务处理并发度，进而影响到整个Job的处理速度。

​	思考：1G的数据，启动8个MapTask，可以提高集群的并发处理能力。那么1K的数据，也启动8个MapTask，会提高集群性能吗？MapTask并行任务是否越多越好呢？哪些因素影响了MapTask并线度？

2、MapTask并行度决定机制

​	数据块：Block是HDFS物理上把数据分成一块一块。

​	数据切片：数据切片只是在逻辑上对输入进行分片，并不会在磁盘上将其切分成片进行存储。

3、数据切片与MapTask并行度决定机制

​	1、一个Job的Map阶段并行度由客户端在提交Job时的切片数决定

​	2、每一个Split切片分配一个MapTask并行实例处理

​	3、默认情况下，切片大小=BlockSize

​	4、切片时不考虑数据集整体，而是逐个针对每一个文件单独切片

#### 3.1.2、Job提交流程源码和切片源码详情

```java
waitForCompletion()

submit();

// 1建立连接
connect();	
// 1）创建提交Job的代理
new Cluster(getConfiguration());
// （1）判断是本地yarn还是远程
initialize(jobTrackAddr, conf); 

// 2 提交job
submitter.submitJobInternal(Job.this, cluster)
// 1）创建给集群提交数据的Stag路径
Path jobStagingArea = JobSubmissionFiles.getStagingDir(cluster, conf);

// 2）获取jobid ，并创建Job路径
JobID jobId = submitClient.getNewJobID();

// 3）拷贝jar包到集群
copyAndConfigureFiles(job, submitJobDir);	
rUploader.uploadFiles(job, jobSubmitDir);

// 4）计算切片，生成切片规划文件
writeSplits(job, submitJobDir);
maps = writeNewSplits(job, jobSubmitDir);
input.getSplits(job);

// 5）向Stag路径写XML配置文件
writeConf(conf, submitJobFile);
conf.writeXml(out);

// 6）提交Job,返回提交状态
status = submitClient.submitJob(jobId, submitJobDir.toString(), job.getCredentials());
```

切片源码解析：

1、程序先找到你数据存储的目录

2、开始遍历处理（规划切片）目录下的每一个文件

3、遍历第一个文件ss.txt  大小300mb

​	a）获取文件大小fs.sizeOf(ss.txt)

​	b）计算切片大小computeSplitSize(Math.max(minSize,Math.min(maxSize,blocksize)))=blocksize=128M

​	c）默认情况下，切片大小=blocksize

​	d）开始切，形成第一个切片：ss.txt-0M:125M 第二个切片ss.txt-128M:256M 第三个切片ss.txt-256M:300M

​	（每次切片时，都要判断切完剩下的部分是否大于块的1.1倍，不大于1.1倍就划分成一块切片）

​	e）将切片信息写到一个切片规划文件中

​	f）整个切片的核心过程在getSplit()方法中完成

​	g）InputSplit只记录了切片的元数据，比如起始位置、长度以及所在的节点列表等。

4、提交切片规划文件到YARN上，YARN上的M人AppMaster就可以根据切片规划文件计算开启MapTask个数。

#### 3.1.3、FileInputFormat切片机制

1、切片机制

- 简单的按照文件的内容长度进行切片

- 切片大小，默认等于Block大小

- 切片时不考虑数据集整体，而是逐个针对每一个文件单独切片

2、FileInputFormat切片大小的参数配置

```java
（1）源码中计算切片大小的公式 
Math.max(minSize, Math.min(maxSize, blockSize)); 
mapreduce.input.fileinputformat.split.minsize= 1 默认值为1
mapreduce.input.fileinputformat.split.maxsize= Long.MAXValue 默认值Long.MAXValue
因此，默认情况下，切片大小=blocksize。

（2）切片大小设置
maxsize（切片最大值）：参数如果调得比blockSize小，则会让切片变小，而且就等于配置的这个参数的值。
minsize（切片最小值）：参数调的比blockSize大，则可以让切片变得比blockSize还大。

（3）获取切片信息API
// 获取切片的文件名称
String name = inputSplit.getPath().getName();
// 根据文件类型获取切片信息
FileSplit inputSplit = (FileSplit) context.getInputSplit();
```

#### 3.1.4、CombineTextInputFormat 切片机制

​	框架默认的TextInputFormat切片机制是对人物按文件规划切片，不管文件多小，都会是一个单独的切片，都会交给一个MapTask，这样如果有大量小文件，就会产生大量的MapTask，处理效率极其低下。

1、应用场景

​	CombinTextInputFormat用于小文件过多的场景，它可以将多个小文件从逻辑上规划到一个切片中，这样，多个小文件就可以交给一个MapTask处理。

2、虚拟存储切片最大值设置

```java
// 设置小文件用的inputFormat
job.setInputFormatClass(CombineTextInputFormat.class);
// 设置虚拟存储切片最大值4M，实际工作用128M
CombineTextInputFormat.setMaxInputSplitSize(job, 4194304);  

// 注意：虚拟存储切片最大值设置最好根据实际的小文件大小情况来设置具体的值
```

3、切片机制

![CombineTextInputFormat 切片机制](大数据md图片\CombineTextInputFormat 切片机制.png)



4、思考

​	在运行MapReduce程序时，输入的文件格式包括：基于行的日志文件、二进制格式文件、数据库表等。Name针对不同的数据类型，MapReduce是如何读取这些数据的呢？

​	FileInputFormat常见的接口实现类包括：TextInputFormat、KeyValueTextInputFormat、NLineInputFormat、CombineTextInputFormat和自定义InputFormat等。

#### 3.1.6、TextInputFormat

​	TextInputFormat是默认的FileInput实现类。按行读取每条记录。键是存储该行在整个文件中的起始字节偏移量，LongWritable类型。值是这行的内容，不包括任何行终止符（换行符和回车符），Text类型。

```java
// 输入格式，默认值
job.setInputFormatClass(TextInputFormat.class);
job.setOutputFormatClass(TextInputFormat.class);
```

#### 3.1.7、KeyValueTextInputFormat

​	每一行均为一条记录，被分隔符分割为key、value。可以通过在驱动类中设定分隔符。默认分隔符是tab（\t）。

```java
// 设置切割符
conf.set(KeyValueLineRecordReader.KEY_VALUE_SEPERATOR, " ");
// 设置输入格式
job.setInputFormatClass(KeyValueTextInputFormat.class);
```

#### 3.1.8、NLineInputFormat

​	如果使用NLineIntputFormat，代表每个map进程处理的InputSplit不再按Block块去划分，而是按NLineInputFormat指定的行数N来划分。技术如文件的总行数/N=切片数，如果不整除，切片数=商+1。

```java
// 设置每3行切1块
NLineInputFormat.setNumLinesPerSplit(job, 3);
// 设置按行切块
job.setInputFormatClass(NLineInputFormat.class);
```

#### 3.1.9、自定义InputFormat

​	在企业开发中，Hadoop框架自带的InputFormat类型不能满足所有应用场景，需要自定义InputFormat来解决实际问题。

自定义InputFormat步骤如下：

1. 自定义一个类继承FileInputFormat。
2. 改写RecordReader，实现一次读取一个完整文件封装为KV。
3. 在输出的时候使用SequenceFileOutputFormat输出合并文件。

程序实现：

1、自定义InputFormat类

```java
public class WholeFileInputformat extends FileInputFormat<Text, BytesWritable>{
	@Override
	public RecordReader<Text, BytesWritable> createRecordReader(InputSplit split, TaskAttemptContext context)
			throws IOException, InterruptedException {
		WholeRecordReader recordReader = new WholeRecordReader();
		recordReader.initialize(split, context);
		return recordReader;
	}
}
```

2、自定义RecordReader类

```java
public class WholeRecordReader extends RecordReader<Text, BytesWritable>{

	FileSplit split;
	Configuration configuration;
	Text k = new Text();
	BytesWritable v = new BytesWritable();
	Boolean isProgress = true;
	
	@Override
	public void initialize(InputSplit split, TaskAttemptContext context) throws IOException, InterruptedException {
		// 初始化
		this.split = (FileSplit) split;
		this.configuration = context.getConfiguration();
	}

	@Override
	public boolean nextKeyValue() throws IOException, InterruptedException {
		// 核心业务逻辑处理
		if(isProgress) {
			// 1 获取fs对象
			Path path = split.getPath();
			FileSystem fs = path.getFileSystem(configuration);
			
			// 2 获取输入流
			FSDataInputStream fis = fs.open(path);
			
			// 3 拷贝
			byte[] buf = new byte[(int) split.getLength()];
			IOUtils.readFully(fis, buf, 0, buf.length);
			
			// 4 封装v
			v.set(buf, 0, buf.length);
			
			// 5 封装k
			k.set(path.toString());
			
			// 6 关闭资源
			IOUtils.closeStream(fis);
			isProgress = false;
			
			return true;
		}
		
		return false;
	}

	@Override
	public Text getCurrentKey() throws IOException, InterruptedException {
		return k;
	}

	@Override
	public BytesWritable getCurrentValue() throws IOException, InterruptedException {
		return v;
	}

	@Override
	public float getProgress() throws IOException, InterruptedException {
		// 获取正在处理的进程，进度条
		return 0;
	}

	@Override
	public void close() throws IOException {	}
}
```

3、编写SequenceFileMapper类处理流程

```java
public class SequenceFileMapper extends Mapper<Text, BytesWritable, Text, BytesWritable>{
	@Override
	protected void map(Text key, BytesWritable value, Context context)
			throws IOException, InterruptedException {
		context.write(key, value);
	}
}
```

4、编写SequenceFileReducer类处理流程

```java
public class SequenceFileReducer extends Reducer<Text, BytesWritable, Text, BytesWritable> {

	@Override
	protected void reduce(Text key, Iterable<BytesWritable> values,
			Context context) throws IOException, InterruptedException {
		
		// 循环写出
		for (BytesWritable value : values) {
			context.write(key, value);
		}
	}
}
```

5、编写SequenceFileDriver类处理流程

```java
public class SequenceFileDriver {
	
	public static void main(String[] args) throws Exception {

		args = new String[] { "D:\\hadoop_input_whole", "D:\\hadoop_output_whole" };

		// 1 获取job对象
		Configuration conf = new Configuration();
		// 设置切割符
		conf.set(KeyValueLineRecordReader.KEY_VALUE_SEPERATOR, " ");

		Job job = Job.getInstance(conf);

		// 2 设置jar路径
		job.setJarByClass(SequenceFileDriver.class);

		// 3 关联mapper和reduce
		job.setMapperClass(SequenceFileMapper.class);
		job.setReducerClass(SequenceFileReducer.class);

		// 8 设置输入格式
		job.setInputFormatClass(WholeFileInputformat.class);
		// 9 设置输出格式
		job.setOutputFormatClass(SequenceFileOutputFormat.class);

		// 4 设置mapper输出的key和value类型
		job.setMapOutputKeyClass(Text.class);
		job.setMapOutputValueClass(BytesWritable.class);

		// 5 设置最终输出的key和value类型
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(BytesWritable.class);

		// 6 设置输入输出路径
		FileInputFormat.setInputPaths(job, new Path(args[0]));
		FileOutputFormat.setOutputPath(job, new Path(args[1]));

		// 7 提交job
		boolean result = job.waitForCompletion(true);
		System.exit(result ? 0 : 1);
	}
}
```

### 3.2、MapReduce 工作流程xxxxxx

少个流程图

### 3.3、Shuffle 机制

#### 3.3.1、Shuffle 机制

​	Map方法之后，Reduce方法之前的数据处理过程称为Shuffle。

#### 3.3.2、Partition分区

1、问题引出

​	要求将统计结果按照条件输出到不同文件中（分区）。比如：将统计结果按照手机归属地不同输出到不同文件中。

2、默认Partitioner分区

​	默认分区是根据key的hashCode对ReduceTasks个数取模得到的。用户没法控制哪个key存储到哪个分区。

```java
public class HashPartitioner<K, V> extends Partitioner<K, V> {
  public int getPartition(K key, V value, int numReduceTasks) {
    return (key.hashCode() & Integer.MAX_VALUE) % numReduceTasks;
  }
}
```

3、自定义Partitioner步骤

1）自定义

```java
public class ProvincePartitioner extends Partitioner<Text, FlowBean> {

	@Override
	public int getPartition(Text key, FlowBean value, int numPartitions) {
		// 控制分区代码逻辑
		return partition;
	}
}
```

2）在job驱动中设置自定义Paritioner

```java
// 关联分区
job.setPartitionerClass(ProvincePartitioner.class);
```

3）自定义Paritioner后，需要根据自定义

4、分区总结

1. 如果 ReduceTask的数量 > getPartition() 的结果数，则会多产生几个空的输出文件part-r000xx
2. 如果 1 < ReductTask的数量 < getPartition() 的结果数，则有一部分分区数据无处安放，会Exception
3. 如果 ReductTask的数量 = 1，则不管MapTask端输出多少个分区文件，最终结果都交给这一个ReduceTask，最终也就只会产生一个结果文件 part-r-0000
4. 分区号必须从0开始，逐一累加

5、案例分析

- 假设：自定义分区数为5，则
- job.setNumReduceTasks(1); 会正常于宁，只不过会产生一个输出文件
- job.setNumReduceTasks(2); 会报错IO异常
- job.setNumReduceTasks(6); 大于5，程序会正常运行，但会产生空文件

#### 3.3.3、Partition分区案例

#### 3.3.4、WritableComparable排序

​	排序是MapReduce框架中最重要的操作之一。

​	MapTask和ReduceTask均会对数据按照key进行排序。该操作数据Hadoop默认行为。任何应用程序中的数据均会被排序，而不管逻辑上是否需要。

​	默认排序是按照字典顺序排序，且实现该排序的方法是快速排序。

##### 3.3.3.0、排序概述

​	对于MapTask，它会将处理的结果暂时放到环形缓冲区中，当环形缓冲区使用率达到一定阈值后，再对缓冲区中的数据进行一次快速排序，并将这些有序数据溢写到磁盘上，而当数据处理完毕后，它会对磁盘上所有文件进行归并排序。

​	对于ReduceTask，它从每个MapTask上远程拷贝相应的数据文件，如果文件大小超过一定阈值，则溢写到磁盘上，否则存储在内存中。如果磁盘上文件数目达到一定阈值，则进行一次归并排序以生成一个更大的文件；如果内存中文件大小或者数目超过一定阈值，则进行一次合并后将数据溢写到磁盘上。当所有数据拷贝完毕后，ReduceTask统一对内存和磁盘上的所有数据进行一次归并排序。

##### 3.3.3.1、排序分类

1、部分排序

​	MapReduce根据输入记录的键值对数据集排序。保证输出的每个文件内部有序。

2、全排序

​	最终输出结果只有一个文件，且文件内部有序。实现方式是只设置一个ReduceTask。但该方法在处理大型文件时效率极低，因为一台机器处理所有文件，完全丧失了MapReduce所提供的并行架构。

3、辅助输出（GroupingComparator分组）

​	在Reduce端对key进行分组。应用于：在接受的key为bean对象时，想让一个或几个字段相同（全部字段比较不相同）的key进入到同一个reduce方法时，可以采用分组排序。

4、二次排序

​	在自定义排序过程中，如果compareTo中的判断条件为两个即为二次排序。

5、自定义排序WritableComparable

​	bean对象作为key传输，需要实现WritableComparable接口重写compareTo方法，就可以实现排序。

```java
@Override
public int compareTo(FlowBean o) {
	int result;
	
	// 按照总流量大小，倒序排列
	if (sumFlow > bean.getSumFlow()) {
		result = -1;
	}else if (sumFlow < bean.getSumFlow()) {
		result = 1;
	}else {
		result = 0;
	}
	return result;
}
```

#### 3.3.5、WritableComparable排序案例实操（全排序）

#### 3.3.6、WritableComparable排序案例实操（区内排序）

#### 3.3.7、Combiner合并

1. Combiner是MR程序中Mapper和Reduce之外的一种组合

2. Combiner组建的父类就是Reducer

3. Combiner和Reduce的区别在于运行位置

   Combiner是在每一个MapTask所在的节电运行

   Reducer是接收全局所有Mapper的输出结果

4. Combiner的意义就是对每一个MapTask的输出进行局部汇总，以减少网络传输量

5. Combiner能够应用的前提下是不能影响最终的业务逻辑，而且，Combiner的输出kv应该跟Reducer的输入kv类型要对应起来

6. 自定义Combiner实现步骤

```java
public class WordcountCombiner extends Reducer<Text, IntWritable, Text, IntWritable>{

	@Override
	protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        // 1 汇总操作
		int count = 0;
		for(IntWritable v :values){
			count += v.get();
		}

        // 2 写出
		context.write(key, new IntWritable(count));
	}
}
```

```java
// 8 设置
job.setCombinerClass(WordcountCombiner.class);
```

#### 3.3.9GroupingComparator分组（辅助排序）

对Reduce阶段的数据根据某一个或几个字段进行分组。

分组排序步骤：

（1）自定义类继承WritableComparator

（2）重写compare()方法

（3）创建一个构造将比较对象的类传给父类

```java
// 1 继承辅助排序
public class OrderGroupingComparator extends WritableComparator{

	// 3 无参构造器
	public OrderGroupingComparator() {
		super(OrderBean.class, true);
	}
	
	// 2 再次分组逻辑
	@Override
	public int compare(WritableComparable a, WritableComparable b) {
		OrderBean aBean = (OrderBean) a;
		OrderBean bBean = (OrderBean) b;
		
		int result;
		if(aBean.getOrderId() > bBean.getOrderId()) {
			result = 1;
		}else if(aBean.getOrderId() < bBean.getOrderId()) {
			result = -1;
		}else {
			result = 0;
		}
		
 		return result;
	}
}
```

### 3.4、MapTask 工作机制

1. Read阶段：MapTask通过用户边写的RecordReader，从输入InputSplit中解析出一个个key/value

2. Map阶段：该节点主要是将解析出的key/value交给用户编写map()函数处理，并产生一系列新的key/value

3. Collect收集阶段：在用户编写map()函数中，当数据处理完成后，一般会调用OutputCollector.collect()输出结果。在该函数内部，它会将生成的key/value分区（调用Partitioner），并写入一个环形内存缓冲区中

4. Spill阶段：即“益写”，当环形缓冲区满后，MapReduce会将数据写到本地磁盘上，生成一个临时文件。需要注意的是，将数据写入本地磁盘之前，先要对数据进行一次本地排序，并在必要时对数据进行合并、压缩等操作。

   益写阶段详情：

   步骤1：利用快速排序算法对缓存区内的数据进行排序，排序方式是，先按照分区编号Partition进行排序，然后按照key进行排序。这样经过排序后，数据以分区为单位聚集在一起，且同一分区内所有数据按照key排序。

   步骤2：按照分区编号由小到大依次将每个分区中的数据写入任务工作目录下的临时文件output/spillN.out(N表示当前益写次数)中。如果用户设置了Combiner，则写入文件之前，对每个分区中的数据进行一次聚集操作。

   步骤3：将分区数据的元信息写到内存索引数据结构SpillRecord中，其中每个分区的元信息包括在临时文件中的偏移量、压缩前数据大小和压缩后数据大小。如果当前内存索引大小超过1MB，则将内存索引写到文件output/spillN.out.index中。

5. Combine阶段：当所有数据处理完成后，MapTask对所有临时文件进行一次合并，以确保最终只会生成一个数据文件。

   当所有数据处理完后，MapTask会将所有临时文件合成一个大文件，并保存到文件output/file.out中，同时生成相应的索引文件output/file.out.index。

   在进行文件合并过程中，MapTask以分区为单位进行合并。对于某个分区，它将采用多轮递归合并的方式。每轮合并io.sort.factor（默认10）个文件，并将产生的文件重新加入待合并列表中，对文件排序后，重复以上过程，直到最终得到一个大文件。

   让每个MapTask最终只生成一个数据文件，可避免同时打开大量文件和同时读取大量小文件产生的随机读取带来的开销。

### 3.5、ReduceTask 工作机制

1. Copy阶段：ReduceTask从各个MapTask上远程拷贝一片数据，并针对某一片数据，如果其大小超过一定阈值，则写到磁盘上，否则无法直接放到内存中。
2. Merge阶段：在远程拷贝数据的同时，ReduceTask启动了两个后台线程对内存和磁盘上的文件进行合并，以防止内存使用过多或磁盘上文件过多。
3. Sort阶段：按照MapReduce语义，用户编写reduce()函数输入数据是按key进行聚集的一组数据。为了将key相同的数据聚集在一起，Hadoop采用了基于排序的策略。由于各个MapTask已经实现对自己的处理结果进行了局部排序，因此，ReduceTask只需对所有数据进行一次归并排序即可。
4. Reduce阶段：reduce()函数将计算结果写到HDFS上。

设置ReduceTask并行度(个数)：

​	ReduceTask的并行度同样影响整个Job的执行并发度和执行效率，但与MapTask的并发数由切片数决定不同，ReduceTask数量的决定是可以直接手动设置。

注意事项：

1. ReduceTask=0，表示没有Reduce阶段，输出文件个数和Map个数一致。
2. ReduceTask默认值就是1，所以输出文件个数为一个。
3. 如果数据分布不均匀，就有可能在Reduce阶段产生数据倾斜。
4. ReduceTask数量并不是任意设置，还要考虑业务逻辑需求，有些情况下，需要计算全局汇总结果，就只能有一个ReduceTask。
5. 具体多少个ReduceTask，需要根据集群性能而定。
6. 如果分区数不是1，但是ReduceTask为1，是否执行分区过程。答案是：不执行分区过程。因为在MapTask的源码中，执行分区的前提是先判断ReduceNum个数是否大于1。不大于1肯定不执行。

### 3.6、OutputFormat 数据输出

#### 3.6.1、OutputFormat接口实现类

​	OutputFormat是MapReduce输出的基类，所有实现MapReduce输出都实现了OutputFormat接口。下面我们介绍几种常见的OutputFormat实现类。

1、文本输出TextOutputFormat

​	默认的输出格式是TextOutputFormat，它把每条记录写为问本行。它的键和值可以是任意类型，因为TextOutputFormat调用toString()方法把它们转换为字符串。

2、SequenceFileOutputFormat

​	将SequenceFileOutputFormat输出作为后续MapReduce任务的输入，这便是一种好的输出格式，因为它的格式紧凑，很容易被压缩。

3、自定义OutputFormat

​	根据用户需求，自定义是线输出。

#### 3.6.2、自定义OutputFormat使用场景及步骤

1、使用场景

​	为了实现控制最终文件的输出路径和输出格式，可以自定义OutputFOrmat。

​	例如：要在一个MapReduce程序中根据数据的不同输出两类结果到不同目录，这类灵活的输出需求可以通过自定义OutputFormat来实现。

2、自定义OutputFormat步骤

​	1）自定义一个类继承FileOutputFormat。

​	2）改写RecordWriter，具体改写输出数据的方法write()。

### 3.7、Join 多种应用

#### 3.7.1、Reduce Join

​	Reduce Join工作原理

​	Map端的主要工作：为来自不同表或文件的key/value对，打标签以区别不同来源的记录。然后用连接字段作为key，其余部分和新加的标志作为value，最后进行输出。

​	Reduce端的主要工作：在Reduce端以连接字段作为key的分组已经完成，我们只需要在每一个分组当中将那些来源于不同文件的记录（在Map阶段已经打标志）分开，最后进行合并就ok了。

​	缺点：这种方式中，合并的操作是在Reduce阶段完成，Reduce端的处理压力太大，Map节点的运算负载则很低，资源利用率不高，且在Reduce阶段极易产生数据倾斜。

​	解决方案：Map端实现数据合并。

#### 3.7.2、Reduce Join案例实操

#### 3.7.3、Map Join

1、使用场景

​	Map Join适用于一表十分小，一张表很大的场景。

2、优点

​	思考：在Reduce端处理过多的表，非常容易产生数据倾斜。怎么办？

​	在Map端缓存多张表，提前处理业务逻辑，这样增加Map端业务，减少Reduce端数据的压力，尽可能的减少数据倾斜。

3、具体办法：采用DistributedCache

​	1）在Mapper的setup阶段，将文件读取到缓存集合中。

​	2）在驱动函数中加载缓存。

#### 3.7.4、Map Join案例实操

### 3.8、计数器应用

### 3.9、数据清洗（ETL）

### 3.10、MapReduce 开发总结



## 第四章 Hadoop数据压缩

### 4.1、概述

### 4.2、MR支持的压缩编码

### 4.3、压缩方式选择

#### 4.3.1、Gzip压缩

#### 4.3.2、Bzip压缩

#### 4.3.3、Lzo压缩

#### 4.3.4、Snappy压缩

### 4.4、压缩位置选择

### 4.5、压缩参数配置

### 4.6、压缩实操案例

#### 4.6.1、数据流的压缩和解压缩

#### 4.6.2、Map输出端采用压缩

#### 4.6.3、Reduce输出端采用压缩

## 第五章 Yarn资源调度器





## 第六章



## 第七章





## 第八章 常见错误及解决方案





高情商的敌人



回家作业：

常用的排序  冒泡排序 快速排序


学hadoop需要画图

分离 和 聚合

##### web端查看HDFS文件系统
http://hadoop101:50070

##### web端查看SecondaryNameNode
http://hadoop103:50090/status.html





进度：188/200   复习

视频容量：6.3G / 12.5G

问题：





电信项目 06/34

## 英语：

context英 ['kɒntekst]  美 ['kɑntɛkst] 

- n. 环境；上下文；来龙去脉

exception英 [ɪk'sepʃ(ə)n; ek-]  美 [ɪk'sɛpʃən] 

- n. 例外；异议

prefix英 ['priːfɪks]  美 ['prifɪks] 

- n. 前缀
- vt. 加前缀；将某事物加在前面

suffix英 ['sʌfɪks]  美 ['sʌfɪks] 

- n. 后缀；下标
- vt. 添后缀

version英 ['vɜːʃ(ə)n]  美 ['vɝʒn] 

- n. 版本；译文；倒转术

share英 [ʃeə]  美 [ʃɛr] 

- vt. 分享，分担；分配
- vi. 共享；分担
- n. 份额；股份
- n. (Share)人名；(阿拉伯)沙雷

example英 [ɪg'zɑːmp(ə)l; eg-]  美 [ɪg'zæmpl] 

- n. 例子；榜样
- vt. 作为…的例子；为…做出榜样
- vi. 举例

exists英  美 

- n. 存在量词（exist的复数）
- v. 存在；出现；活着（exist的三单形式）

bash英 [bæʃ]  美 [bæʃ] 

- vt. 猛击，痛击；怒殴
- n. 猛烈的一击，痛击
- n. (Bash)人名；(英、俄、巴基)巴什

chmod英  美 

- n. 更改文件属性；档案权限，修改文件权限；改变文件存取方式

echo英 ['ekəʊ]  美 ['ɛko] 

- vt. 反射；重复
- vi. 随声附和；发出回声
- n. 回音；效仿

property英 ['prɒpətɪ]  美 ['prɑpɚti] 

- n. 性质，性能；财产；所有权

slaves英 [sleɪvz]  美 [slevz] 

- n. 奴隶, 服伺的人; 侍女; 上瘾; 服务于另一连接计算机的系统 (计算机用语)

reduce英 [rɪ'djuːs]  美 [rɪ'dʊs] 

- vt. 减少；降低；使处于；把…分解
- vi. 减少；缩小；归纳为

secondary英 ['sek(ə)nd(ə)rɪ]  美 ['sɛkəndɛri] 

- adj. 第二的；中等的；次要的；中级的
- n. 副手；代理人

module英 ['mɒdjuːl]  美 ['mɑdʒul] 

- n. [计] 模块；组件；模数

software英 ['sɒf(t)weə]  美 ['sɔftwɛr] 

- n. 软件

profile英 ['prəʊfaɪl]  美 ['profaɪl] 

- n. 侧面；轮廓；外形；剖面；简况
- vt. 描…的轮廓；扼要描述
- vi. 给出轮廓

status英 ['steɪtəs]  美 [ˈstetəs] 

- n. 地位；状态；情形；重要身份

daemon英 ['diːmən]  美 ['dimən] 

- n. 守护进程；后台程序

encoding英 [ɪn'kəʊdɪŋ]  美 [ɪn'kodɪŋ] 

- n. [计] 编码
- v. [计] 编码（encode的ing形式）

install英 [ɪnˈstɔ:l]  美 [ɪn'stɔl] 

- vt. 安装；任命；安顿

distributed英 [dɪ'strɪbjʊtɪd]  美 [dɪ'strɪbjʊtɪd] 

- adj. 分布式的，分散式的

master英 ['mɑːstə]  美 ['mæstɚ] 

- vt. 控制；精通；征服
- n. 硕士；主人；大师；教师
- adj. 主人的；主要的；熟练的
- n. (Master)人名；(英)马斯特

dependency英 [dɪ'pend(ə)nsɪ]  美 [dɪ'pɛndənsi] 

- n. 属国；从属；从属物

[ 复数 dependencies ] 

artifact英 ['ɑ:təˌfækt]  美 ['ɑrtə,fækt] 

- n. 人工制品；手工艺品

configuration英 [kən,fɪgə'reɪʃ(ə)n; -gjʊ-]  美 [kən,fɪɡjə'reʃən] 

- n. 配置；结构；外形

iterator英 [ɪtə'reɪtə]  美 [ɪtə'retɚ] 

- n. 迭代器；迭代程序

location英 [lə(ʊ)'keɪʃ(ə)n]  美 [lo'keʃən] 

- n. 位置（形容词locational）；地点；外景拍摄场地

block英 [blɒk]  美 [blɑk] 

- n. 块；街区；大厦；障碍物
- vt. 阻止；阻塞；限制
- adj. 成批的，大块的；交通堵塞的
- n. (Block)人名；(英、法、德、西、葡、芬、罗)布洛克

stream英 [striːm]  美 [strim] 

- n. 溪流；流动；潮流；光线
- vi. 流；涌进；飘扬
- vt. 流出；涌出；使飘动

task英 [tɑːsk]  美 [tæsk] 

- vt. 分派任务
- n. 工作，作业；任务

writable['raɪtəbl] 

- adj. 可写的，能写成文的

common英 ['kɒmən]  美 ['kɑmən] 

- adj. 共同的；普通的；一般的；通常的
- n. 普通；平民；公有地
- n. (Common)人名；(法)科蒙；(英)康芒

[ 比较级 commoner 最高级 commonest ]

extends英 [ɪk'stendz; ek-]  美 [ɪk'stɛndz] 

- v. 延伸；扩充；继承（extend的第三人称单数形式）

override英 [əʊvə'raɪd]  美 [,ovɚ'raɪd] 

- vt. 推翻；不顾；践踏
- n. 代理佣金

[ 过去式 overrode 过去分词 overridden 现在分词 overriding ]

overwrite英 [əʊvə'raɪt]  美 [,ovɚ'raɪt] 

- vt. 重写；写得过多；重叠写在…上面
- vi. 写得过多或过长
- n. 重写；（美）代理佣金

[ 过去式 overwrote 过去分词 overwritten 现在分词 overwriting ]

completion英 [kəm'pliːʃn]  美 [kəm'pliʃən] 

- n. 完成，结束；实现

plugin英 [plʌgɪn]  美 [plʌgɪn] 

- n. 插件；相关插件
- n. (Plugin)人名；(俄)普卢金

compare英 [kəm'peə]  美 [kəm'pɛr] 

- n. 比较
- vt. 比拟，喻为；[语]构成
- vi. 相比，匹敌；比较，区别；比拟（常与to连用）
- n. (Compare)人名；(意)孔帕雷

comparable英 ['kɒmp(ə)rəb(ə)l]  美 ['kɑmpərəbl] 

- adj. 可比较的；比得上的

sequence英 ['siːkw(ə)ns]  美 ['sikwəns] 

- n. [数][计] 序列；顺序；续发事件
- vt. 按顺序排好

[ 过去式 sequenced 过去分词 sequenced 现在分词 sequencing ]

shuffle英 ['ʃʌf(ə)l]  美 ['ʃʌfl] 

- v. 洗牌；推诿，推卸；拖曳，慢吞吞地走；搅乱
- n. 洗牌，洗纸牌；混乱，蒙混；拖着脚走

[ 过去式 shuffled 过去分词 shuffled 现在分词 shuffling ]

partition英 [pɑː'tɪʃ(ə)n]  美 [pɑr'tɪʃən] 

- n. 划分，分开；[数] 分割；隔墙；隔离物
- vt. [数] 分割；分隔；区分

partitioner[pa:'tiʃənə] 

- n. 瓜分者，分割者

province英 ['prɒvɪns]  美 ['prɑvɪns] 

- n. 省；领域；职权



