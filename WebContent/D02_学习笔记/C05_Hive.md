# 尚硅谷大数据技术之Hive

# 第一章、基本概念

## 1、什么是Hive

Hive：由Facebook开源用于解决海量结构化日志的数据统计。

Hive是基于Hadoop的一个数据仓库，可以将结构化的数据文件映射为一张表，并提供类SQL查询功能。

本质是：将HQL转化成MapReduce程序。

![sql-mapreduce](大数据md图片\sql-mapreduce.png)

1）Hive处理的数据存储在HDFS

2）Hive分析数据底层的是现实MapReduce

3）执行程序运行在Yarn商

## 2、Hive的优缺点

### 2.1、优点

1）操作接口采用类SQL语法，提供快速开发的能力（简单、容易上手）。

2）避免了去写MapReduce，减少开发人员的学习成本。

3）Hive的执行延迟比较高，因此Hive常用于数据分析，对实时性要求不高的场合。

4）Hive优势在于处理大数据，对于处理小数据没有优势，因为Hive的执行延迟比较高。

5）Hive支持用户自定义函数，用户可以根据自己的需求来实现自己的函数。

### 2.2、缺点

Hive的HQL表达能力有限

​	1）迭代式算法无法表达

​	2）数据挖掘方面不擅长

Hive的效率比较低

​	1）Hive自动生成的MapReduce作业，通常情况下不够智能化

​	2）Hive调优比较困难，粒度较粗

## 3、Hive架构原理

![Hive架构原理](大数据md图片\Hive架构原理.png)

1、用户借口：Client

CLI（Hive shell）、JDBC/ODBC（java访问hive）、WEBUI（浏览器访问hive）

2、元数据：Metastore

元数据包括：表名、表所属的数据库（默认是default）、表的拥有者、列/分区字段、表的类型（是否是外部表）、表的数据所在目录等

默认存储在自带的derby数据库中，推荐使用MySQL存储Metastore

3、Hadoop

使用HDFS进行存储，使用MapReduce进行计算

4、驱动器：Driver

1）解析器（SQL Parser）：将SQL字符串转换成抽象语法树AST，这一步一般都用第三方工具库完成，比如antlr；对AST进行语法分析，比如表是否存在、字段是否存在、SQL语义是否有误。

2）编译器（Physical Plan）：将AST编译生成逻辑执行计划

3）优化器（Query Optimizer）：对逻辑执行计划进行优化

4）执行器（Execution）：把逻辑执行计划转换成可以运行的物理计划。对于Hive来说，就是MR/Spark。

![hive运行机制](大数据md图片\hive运行机制.png)

Hive通过给用户提供的一系列交互接口，收到用户的指令（SQL），使用自己的Driver，结合元数据（MetaStore），将这些指令翻译成MapReduce，提交到Hadoop中执行，最后，将执行返回的结果输出到用户交互接口。

## 4、Hive和数据库比较

由于Hive采用了类似SQL的查询语言HQL（Hive Query Language），因此很容易将Hive理解为数据库。其实从结构上来看，Hive和数据库除了拥有类似的查询语言，再无类似之处。本文将从多方面来阐述Hive和数据库的差异。数据库可以用在Online的应用中，但是Hive是为数据仓库而设计的，清楚这一点，有助于从应用角度理解Hive的特性。

### 4.1、查询语言

由于SQL被广泛的应用在数据仓库中，因此，专门针对Hive的特性设计了类SQL的查询语言HQL。熟悉SQL开发的开发者可以很方便的使用Hive进行开发。

### 4.2、数据存储位置

Hive是建立在Hadoop之上的，所有Hive的数据都是存储在HDFS中的。而数据库则可以将数据保存在块设备或本地文件系统中。

### 4.3、数据更新

由于Hive是针对数据仓库应用设计的，而数据仓库的内容是读多写少的。因此，Hive中不建议对数据的改写，所有的数据都是在加载的时候确定好的。而数据库中的数据通常是需要经常修改的，因此可以使用INSERT INTO ... VALUES 添加数据，使用UPDATE ... SET 修改数据。

### 4.4、索引

Hive在加载数据的过程中，不会对数据进行任何处理，甚至不会对数据进行扫描，因此也没有对数据中的某些Key建立索引。Hive要访问数据中满足条件的特定值时，需要暴力扫描整个数据，因此访问延迟较高。由于MapReduce的引入，Hive可以并行访问数据，因此即使没有索引，对于大数据量的访问，Hive仍然可以体现出优势。数据库中，通常会针对一个或者几个列建立索引，因此对于少量的特定条件的数据的访问，数据库可以由很高的效率，较低的延迟。由于数据的访问延迟较高，决定了Hive不适合在线数据查询。

### 4.5、执行

Hive中大对数查询的执行是通过Hadoop提供的MapReduce来实现的。而数据库通常有自己的执行引擎。

### 4.6、执行延迟

Hive在查询数据的时候，由于没有索引，需要扫描整个表，因此延迟较高。另外一个导致Hive执行延迟高的因素是MapReduce框架。由于MapReduce本身具有较高的延迟，因此在利用MapReduce执行Hive查询时，也会有较高的延迟。相对的，数据库的执行延迟较低。当然，这个低是有条件的，即数据规模较小，当数据规模大到超过数据库的处理能力的时候，Hive的并行计算显然能体现出优势。

### 4.7、可扩展性

由于Hive是建立在Hadoop之上的，因此Hive的可扩展性是和Hadoop的扩展性是一致的（世界上最大的Hadoop集群在Yahoo!，2009年的规模在4000台节点左右）。而数据库由于ACID语义的严格控制，扩展行非常有限。目前最先进的并行数据库Oracle在理论上的扩展能力也只有100台左右。

### 4.8、数据规模

由于Hive建立在集群商并可以利用MapReduce进行并行计算，因此可以支持很大规模的数据。对应的，数据库可以支持的数据规模较小。

# 第二章、Hive安装

## 1、Hive下载地址

1）Hive官网地址

http://hive.apache.org/

2）文档查看地址

https://cwiki.apache.org/confluence/display/Hive/GettingStarted

3）下载地址

http://archive.apache.org/dist/hive/

4）github地址

https://github.com/apache/hive

## 2、Hive安装部署

### 2.1、Hive安装及配置

```sh
# 1 复制文件到linux
apache-hive-1.2.1-bin.tar.gz   mysql-libs.zip

# 2 解压
tar -zxvf apache-hive-1.2.1-bin.tar.gz -C /opt/module/

# 3 配置文件
cd /opt/module/apache-hive-1.2.1-bin/conf
mv hive-env.sh.template hive-env.sh

# 4 配置参数
vim hive-env.sh
HADOOP_HOME=/opt/module/hadoop-2.7.2
export HIVE_CONF_DIR=/opt/module/hive/conf

# 5 环境变量
export HADOOP_CLASSPATH=$HIVE_HOME/conf
export HADOOP_CLASSPATH=$HADOOP_CLASSPATH:$HIVE_HOME/lib

source /etc/profile
```

### 2.2、Hadoop集群配置

```sh
# 1 必须启动hdfs和yarn
cd /opt/module/hadoop-2.7.2
sbin/start-dfs.sh
sbin/start-yarn.sh

# 2 在HDFS上创建/tmp和/user/hive/warehouse两个目录
bin/hadoop fs -mkdir /tmp
bin/hadoop fs -mkdir -p /user/hive/warehouse

# 3 修改同组权限可写
bin/hadoop fs -chmod g+w /tmp
bin/hadoop fs -chmod g+w /user/hive/warehouse
```

### 2.3、Hive基本操作

```sh
# 1 启动Hive
bin/hive

# 2 查看数据库
hive> show databases;

# 3 打开默认数据库
hive> use default;

# 4 创建一张表
hive> create table student(id int, name string);

# 5 查看数据库中的表
hive> show tables;

# 6 查看表结构
hive> desc student;

# 7 向表中插入一条数据
hive> insert into student values(1000,"ss");

# 8 查询表中数据
hive> select * from student;

# 9 退出Hive
hive> quit;
```

## 3、将本地文件导入Hive案例

需求：将本地/opt/module/datas/student.txt这个目录下的数据导入hive的student表中。

### 3.1、准备数据

```sh
# 1 在/opt/module/目录下创建datas
mkdir datas

# 2 在/opt/module/datas/目录下创建student.txt文件并添加数据
vi student.txt

1001	zhangshan
1002	lishi
1003	zhaoliu

# 注意以tab键间隔。
```

### 3.2、Hive实际操作

```sh
# 1 启动hive
bin/hive

# 2 显示数据库
hive> show databases;

# 3 使用default数据库
hive> use default;

# 4 显示default数据库中的表
hive> show tables;

# 5 删除已创建的student表
hive> drop table student;

# 6 创建student表, 并声明文件分隔符’\t’
hive> create table student(id int, name string) ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t';

# 7 加载/opt/module/datas/student.txt 文件到student数据库表中。
hive> load data local inpath '/opt/module/datas/student.txt' into table student;

# 8 Hive查询结果
hive> select * from student;
OK
1001	zhangshan
1002	lishi
1003	zhaoliu
Time taken: 0.266 seconds, Fetched: 3 row(s)
```

## 4、MySql安装

### 4.1、安装包准备

```sh
# 1 查看mysql是否安装，如果安装了，卸载mysql
rpm -qa | grep -i mysql
如果有 mysql-libs-5.1.73-7.el6.x86_64
/usr/share/mysql
# 2 卸载
rpm -e --nodeps mysql-libs-5.1.73-7.el6.x86_64

# 3 解压mysql-libs.zip文件到当前目录
unzip mysql-libs.zip

# 4 进入到mysql-libs文件夹下
cd /opt/software/mysql-libs

[root@hadoop102 mysql-libs]# ll
-rw-r--r--. 1 root root 18509960 3月  26 2015 MySQL-client-5.6.24-1.el6.x86_64.rpm
-rw-r--r--. 1 root root  3575135 12月  1 2013 mysql-connector-java-5.1.27.tar.gz
-rw-r--r--. 1 root root 55782196 3月  26 2015 MySQL-server-5.6.24-1.el6.x86_64.rpm
```

### 4.2、安装MySql服务器

```sh
# 安装环境：CentOS7 64位 MINI版，安装MySQL5.7
# 1 下载mysql源安装包
shell> wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

# 2 安装mysql源
shell> yum localinstall mysql57-community-release-el7-8.noarch.rpm

# 3 检查mysql源是否安装成功
shell> yum repolist enabled | grep "mysql.*-community.*"

mysql-connectors-community/x86_64 MySQL Connectors Community                 108
mysql-tools-community/x86_64      MySQL Tools Community                       90
mysql57-community/x86_64          MySQL 5.7 Community Server                 347
# 看到上述表示安装成功。

# 4 安装MySQL服务端
shell> yum install mysql-community-server

# 5 启动MySQL服务端
shell> systemctl start mysqld

# 6 查看MySQL的启动状态
shell> systemctl status mysqld

● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 三 2019-05-29 21:20:18 CST; 14min ago

# 7 开机启动
shell> systemctl enable mysqld
shell> systemctl daemon-reload

# 8 查看密码策略的相关信息
mysql> show variables like '%password%';

validate_password_policy：密码策略，默认为MEDIUM策略 
validate_password_dictionary_file：密码策略文件，策略为STRONG才需要 
validate_password_length：密码最少长度 
validate_password_mixed_case_count：大小写字符长度，至少1个 
validate_password_number_count ：数字至少1个 
validate_password_special_char_count：特殊字符至少1个 
上述参数是默认策略MEDIUM的密码检查规则。

共有以下几种密码策略：
策略	      检查规则
0 or LOW	Length
1 or MEDIUM	Length; numeric, lowercase/uppercase, and special characters
2 or STRONG	Length; numeric, lowercase/uppercase, and special characters; dictionary file

# 9 修改密码策略
在/etc/my.cnf文件添加validate_password_policy配置，指定密码策略

# 选择0（LOW），1（MEDIUM），2（STRONG）其中一种，选择2需要提供密码字典文件
validate_password_policy=0
# 如果不需要密码策略，添加my.cnf文件中添加如下配置禁用即可：
validate_password=off

重新启动mysql服务使配置生效：
systemctl restart mysqld

# 10 配置默认编码为utf8
修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：

[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'


修改vim /etc/yum.repos.d/mysql-community.repo源，改变默认安装的mysql版本。
# 比如要安装5.6版本，将5.7源的enabled=1改成enabled=0。然后再将5.6源的enabled=0改成enabled=1即可。
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql


1．安装mysql服务端
[root@hadoop102 mysql-libs]# rpm -ivh MySQL-server-5.6.24-1.el6.x86_64.rpm
2．查看产生的随机密码
[root@hadoop102 mysql-libs]# cat /root/.mysql_secret
OEXaQuS8IWkG19Xs
3．查看mysql状态
[root@hadoop102 mysql-libs]# service mysql status
4．启动mysql
[root@hadoop102 mysql-libs]# service mysql start
```

### 4.3、安装MySql客户端

```sh
# 1 修改root本地登录密码
# mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码。通过下面的方式找到root默认密码，然后登录mysql进行修改：
shell> grep 'temporary password' /var/log/mysqld.log

shell> mysql -uroot -p
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '1234'; 

# 2 客户端连接mysql
shell> mysql -uroot -p1234
```

### 4.4、MySql中user表中主机配置

```sh
# 配置只要是root用户+密码，在任何主机上都能登录MySQL数据库。
# 1 进入mysql
shell> mysql -uroot -p1234

# 2 显示数据库
mysql> show databases;

# 3 使用mysql数据库
mysql> use mysql;

# 4 展示mysql数据库中的所有表
mysql> show tables;

# 5 展示user表的结构
mysql> desc user;

# 6 查询user表
mysql> select User, Host, authentication_string from user;

# 7 修改user表，把Host表内容修改为%
mysql> update user set host='%' where host='localhost';

# 8 删除root用户的其他host
mysql> delete from user where Host='hadoop102';
mysql> delete from user where Host='127.0.0.1';
mysql> delete from user where Host='::1';

# 9 刷新
mysql> flush privileges;

# 10 退出
mysql> quit;
```

## 5、Hive元数据配置到MySql

### 5.1、驱动拷贝

```sh
# 1 驱动拷贝
tar -zxvf mysql-connector-java-5.1.27.tar.gz
cp mysql-connector-java-5.1.27-bin.jar /opt/module/apache-hive-1.2.1-bin/lib/
```

### 5.2、配置Metastore到MySql

```sh
# 1 在/opt/module/hive/conf目录下创建一个hive-site.xml
cd /opt/module/apache-hive-1.2.1-bin/conf
vim hive-site.xml

# 2 根据官方文档配置参数，拷贝数据到hive-site.xml文件中
# https://cwiki.apache.org/confluence/display/Hive/AdminManual+MetastoreAdmin

<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
	<property>
	  <name>javax.jdo.option.ConnectionURL</name>
	  <value>jdbc:mysql://hadoop102:3306/metastore?createDatabaseIfNotExist=true</value>
	  <description>JDBC connect string for a JDBC metastore</description>
	</property>

	<property>
	  <name>javax.jdo.option.ConnectionDriverName</name>
	  <value>com.mysql.jdbc.Driver</value>
	  <description>Driver class name for a JDBC metastore</description>
	</property>

	<property>
	  <name>javax.jdo.option.ConnectionUserName</name>
	  <value>root</value>
	  <description>username to use against metastore database</description>
	</property>

	<property>
	  <name>javax.jdo.option.ConnectionPassword</name>
	  <value>000000</value>
	  <description>password to use against metastore database</description>
	</property>
</configuration>

# 3 配置完毕后，如果启动hive异常，可以重新启动虚拟机。（重启后，别忘了启动hadoop集群）
shell> bin/hive
```

```sh
# hive help帮助
shell> bin/hive -help
usage: hive
 -d,--define <key=value>          Variable subsitution to apply to hive
                                  commands. e.g. -d A=B or --define A=B
    --database <databasename>     Specify the database to use
 -e <quoted-query-string>         SQL from command line
 -f <filename>                    SQL from files
 -H,--help                        Print help information
    --hiveconf <property=value>   Use value for given property
    --hivevar <key=value>         Variable subsitution to apply to hive
                                  commands. e.g. --hivevar A=B
 -i <filename>                    Initialization SQL file
 -S,--silent                      Silent mode in interactive shell
 -v,--verbose                     Verbose mode (echo executed SQL to the console)
```



### 5.3、多窗口启动Hive测试



## 6、HiveJDBC访问

### 6.1、

### 6.2、

### 6.3、

## 7、Hive常用交互命令

## 8、Hive其他命令操作

## 9、Hive常见属性配置

### 9.1、

### 9.2、

### 9.3、

### 9.4、

# 第三章、

## 1、

## 2、

## 3、

## 4、

## 5、

# 第四章、

## 1、

## 2、

## 3、

## 4、

## 5、

# 第五章、

## 1、

## 2、

## 3、

## 4、

## 5、

# 第六章、

## 1、

## 2、

## 3、

## 4、

## 5、

# 第七章、

# 第八章、

# 第九章、

# 第十章、

# 第十一章、

## 1、

1）

2）

3）

4）

5）

## 2、



## 3、

## 4、

## 5、



```sh
# 1


# 2


# 3


# 4


# 5


# 6


# 7


# 8


# 9


```





视频下一个看 10/53