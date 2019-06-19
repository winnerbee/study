# js尚硅谷大数据技术之Hive

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
vim /etc/profile
export HIVE_HOME=/opt/module/hive
export PATH=$PATH:$HIVE_HOME/bin
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

### 5.3、多窗口启动Hive测试

```sh
# 1 启动MySQL
mysql -uroot -p1234

# 2 查看有几个数据库
mysql> show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql             |
| performance_schema |
| test               |
+--------------------+

# 3 打开多个窗口，分别启动hive
shell> bin/hive

# 4 启动hive后，回到MySQL窗口查看数据库，显示增加了metastore数据库
mysql> show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| metastore          |
| mysql             |
| performance_schema |
| test               |
+--------------------+
```

## 6、HiveJDBC访问

### 6.1、启动HiveServer2服务

```sh
[atguigu@hadoop102 hive]$ bin/hiveserver2
```

### 6.2、启动beeline

```sql
[atguigu@hadoop102 hive]$ bin/beeline
Beeline version 1.2.1 by Apache Hive
beeline>
```

### 6.3、连接hiveserver2

```sh
beeline> !connect jdbc:hive2://hadoop102:10000（回车）
Connecting to jdbc:hive2://hadoop102:10000
Enter username for jdbc:hive2://hadoop102:10000: atguigu（回车）
Enter password for jdbc:hive2://hadoop102:10000: （直接回车）
Connected to: Apache Hive (version 1.2.1)
Driver: Hive JDBC (version 1.2.1)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://hadoop102:10000> show databases;
+----------------+--+
| database_name  |
+----------------+--+
| default        |
| hive_db2       |
+----------------+--+
```

## 7、Hive常用交互命令

```sh
[jay@hadoop101 apache-hive-1.2.1-bin]$ bin/hive -help
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


# 1 “-e”不进入hive的交互窗口执行sql语句
bin/hive -e "select id from student;"
# 2 “-f”执行脚本中sql语句
	（1）在/opt/module/datas目录下创建hivef.sql文件
		touch hivef.sql
		文件中写入正确的sql语句 select *from student;
	（2）执行文件中的sql语句
		bin/hive -f /opt/module/datas/hivef.sql
	（3）执行文件中的sql语句并将结果写入文件中
		bin/hive -f /opt/module/datas/hivef.sql  > /opt/module/datas/hive_result.txt
```

## 8、Hive其他命令操作

```SH
# 1 退出hive窗口：
	exit:先隐性提交数据，再退出；
	quit:不提交数据，退出；
	在新版的hive中没区别了，在以前的版本是有的：

# 2 在hive cli命令窗口中如何查看hdfs文件系统
	hive(default)>dfs -ls /;

# 3 在hive cli命令窗口中如何查看本地文件系统
	hive(default)>! ls /opt/module/datas;

# 4 查看在hive中输入的所有历史命令
	（1）进入到当前用户的根目录/root或/home/atguigu
	（2）查看. hivehistory文件
	cat .hivehistory
```

## 9、Hive常见属性配置

### 9.1、Hive数据仓库位置配置

1、Default数据仓库最原始的位置是在hdfs上的 /user/hive/warehouse 路径下。

2、在仓库目录下，没有对默认的数据库default创建文件夹。如果某张表属于default数据库，直接在数据仓库目录下创建一个文件夹。

3、修改default数据仓库原始位置（将hive-default.xml.template如下配置拷贝到hive-site.xml文件中）。

```sh
<property>
	<name>hive.metastore.warehouse.dir</name>
	<value>/user/hive/warehouse</value>
	<description>location of default database for the warehouse</description>
</property>

# 配置同组用户有执行权限
bin/hdfs dfs -chmod g+w /user/hive/warehouse
```

### 9.2、查询后信息显示配置

1、在hive-site.xml文件中添加如下配置信息，就可以实现显示当前数据库，以及查询表的头信息配置。

```xml
<property>
	<name>hive.cli.print.header</name>
	<value>true</value>
</property>

<property>
	<name>hive.cli.print.current.db</name>
	<value>true</value>
</property>
```

2、重新启动hive，对比配资前后差异

### 9.3、Hive运行日志信息配置

1、hive的log默认存放在 /tmp/{user.name}/hive.log 目录下（当前用户名）

2、修改hive的log存放日志到 /opt/module/hive/logs

```sh
# 1 修改/opt/module/hive/conf/hive-log4j.properties.template文件名称为
hive-log4j.properties

# 2 复制文件 mv hive-log4j.properties.template hive-log4j.properties
# 在hive-log4j.properties文件中修改log存放位置
hive.log.dir=/opt/module/hive/logs
```

### 9.4、参数配置方式

1、查看当前所有的配置信息

```
hive> set;
```

2、参数的配置三种方式

```sh
# 1 配置文件方式
默认配置文件：hive-default.xml 
用户自定义配置文件：hive-site.xml
	注意：用户自定义配置会覆盖默认配置。另外，Hive也会读入Hadoop的配置，因为Hive是作为Hadoop的客户端启动的，Hive的配置会覆盖Hadoop的配置。配置文件的设定对本机启动的所有Hive进程都有效。
# 2 命令行参数方式
启动Hive时，可以在命令行添加-hiveconf param=value来设定参数。
例如：
[atguigu@hadoop103 hive]$ bin/hive -hiveconf mapred.reduce.tasks=10;
注意：仅对本次hive启动有效
查看参数设置：
hive (default)> set mapred.reduce.tasks;
# 3 参数声明方式
可以在HQL中使用SET关键字设定参数
例如：
hive (default)> set mapred.reduce.tasks=100;
注意：仅对本次hive启动有效。
查看参数设置
hive (default)> set mapred.reduce.tasks;
```

上述三种设定方式的优先级依次递增。即配置文件<命令行参数<参数声明。注意某些系统级的参数，例如log4j相关的设定，必须用前两种方式设定，因为那些参数的读取在会话建立以前已经完成了。

# 第三章、Hive数据类型

## 1、基本数据类型

| Hive数据类型 | Java数据类型 | 长度                                                   | 例子       |
| ------------ | ------------ | ------------------------------------------------------ | ---------- |
| TINYINT      | byte         | 1byte 有符号整数                                       | 20         |
| SMALINT      | short        | 2byte 有符号整数                                       | 20         |
| INT          | int          | 4byte 有符号整数                                       | 20         |
| BIGINT       | long         | 8byte 有符号整数                                       | 20         |
| BOOLEAN      | boolean      | 布尔类型，true或false                                  | true false |
| FLOAT        | float        | 单精度浮点数                                           | 3.14159    |
| DOUBLE       | double       | 双精度浮点数                                           | 3.14159    |
| STRING       | string       | 字符系列。可以指定字符集。<br>可以使用单引号或双引号。 | 'aa'  "bb" |
| TIMESTAMP    |              | 时间类型                                               |            |
| BINARY       |              | 字节数组                                               |            |

对于Hive的String类型相当于数据库的varchar类型，该类型是一个可变的字符串，不过它不能声明其中最多能存储多少个字符，理论上它可以存储2GB的字符数。

## 2、集合数据类型

- STRUCT：和c语言中的struct类似，都可以通过“点”符号访问元素内容。例如，如果某个列的数据类型是STRUCT{first STRING, last STRING},那么第1个元素可以通过字段.first来引用。
- MAP：MAP是一组键-值对元组集合，使用数组表示法可以访问数据。例如，如果某个列的数据类型是MAP，其中键->值对是’first’->’John’和’last’->’Doe’，那么可以通过字段名[‘last’]获取最后一个元素。
- ARRAY：数组是一组具有相同类型和名称的变量的集合。这些变量称为数组的元素，每个数组元素都有一个编号，编号从零开始。例如，数组值为[‘John’, ‘Doe’]，那么第2个元素可以通过数组名[1]进行引用。

Hive有三种复杂数据类型ARRAY、MAP和STRUCT。ARRAY和MAP和Java中的Array和Map类似，而STRUCT与C语言中的Struct类似，它封装了一个命名字段集合，复杂数据类型允许任意层次的嵌套。

## 3、类型转化

​	Hive的原子数据类型是可以进行隐式转换的，类似于Java的类型转换，例如某表达式使用INT类型，TINYINT会自动转换为INT类型，但是Hive不会进行反向转化，例如，某表达式使用TINYINT类型，INT不会自动转换为TINYINT类型，它会返回错误，除非使用CAST操作。

1、隐式类型转换规则如下

​	（1）任何整数类型都可以隐式地转换为一个范围更广的类型，如TINYINT可以转换成INT，INT可以转换成BIGINT。

​	（2）所有整数类型、FLOAT和STRING类型都可以隐式地转换成DOUBLE。

​	（3）TINYINT、SMALLINT、INT都可以转换为FLOAT。

​	（4）BOOLEAN类型不可以转换为任何其它的类型。

2、可以使用CAST操作显示进行数据类型转换

​	例如CAST('1' AS INT)将把字符串'1' 转换成整数1；如果强制类型转换失败，如执行CAST('X' AS INT)，表达式返回空值 NULL。

# 第四章、DDL数据定义

## 1、创建数据库

1、创建一个数据库，数据库在HDFS商的默认存储路径是 /user/hive/warehouse/*.db

```sh
hive (default)> create database db_hive;
```

2、避免要创建的数据库已经存在错误，增加if not exists判断（标准写法）

```
hive (default)> create database if not exists db_hive;
```

3、创建一个数据库，制定数据库在HDFS上存放的位置

```
hive (default)> create database db_hive3 location '/db_hive3.db';
```

## 2、查询数据库

### 2.1、显示数据库

1、显示数据库

```
hive (default)> show databases;
```

2、过滤显示查询的数据库

```
hive (default)> show databases like 'db_hive*';
```

### 2.2、查看数据库详情

1、显示数据库信息

```js
hive (default)> desc database db_hive;
OK
db_name	comment	location	owner_name	owner_type	parameters
db_hive		hdfs://hadoop101:9000/user/hive/warehouse/db_hive.db	jay	USER	
Time taken: 0.098 seconds, Fetched: 1 row(s)
```

2、显示数据库详细信息，extended

```js
hive (default)> desc database extended db_hive;
OK
db_name	comment	location	owner_name	owner_type	parameters
db_hive		hdfs://hadoop101:9000/user/hive/warehouse/db_hive.db	jay	USER	
Time taken: 0.096 seconds, Fetched: 1 row(s)
```

### 2.3、切换当前数据库

1、切换当前数据库

```
hive (default)> use db_hive;
```

## 3、修改数据库

用户可以使用ALTER DATABASE 命令为某个数据库的 DBPROPERTIES 设置 键-值对属性值，来描述这个数据库的属性信息。数据库的其他元数据信息都是不可更改的，包括数据库名和数据库所在的目录位置。

```js
alter database db_hive set dbproperties('createtime'='20190605');
alter database db_hive set dbproperties('level'='top1');
```

在hive中查看修改结果

```js
hive (default)> desc database extended db_hive;
OK
db_name	comment	location	owner_name	owner_type	parameters
db_hive		hdfs://hadoop101:9000/user/hive/warehouse/db_hive.db	jay	USER	{createtime=20190605, level=top1}
Time taken: 0.103 seconds, Fetched: 1 row(s)
```

## 4、删除数据库

1、删除空数据库

```js
hive (default)> drop database db_hive3;
```

2、如果删除的数据库不存在，最好采用 if exists 判断数据库是否存在

```
hive (default)> drop database if exists  db_hive2;
```

3、如果数据库不为空，可以采用cascade命令，强制删除

```sh
hive (default)> drop database db_hive;
FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. InvalidOperationException(message:Database db_hive is not empty. One or more tables exist.)

# 强制删除
hive (default)> drop database db_hive cascade;
```

## 5、创建表

1、建表语句

```
CREATE [EXTERNAL] TABLE [IF NOT EXISTS] table_name 
[(col_name data_type [COMMENT col_comment], ...)] 
[COMMENT table_comment] 
[PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)] 
[CLUSTERED BY (col_name, col_name, ...) 
[SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS] 
[ROW FORMAT row_format] 
[STORED AS file_format] 
[LOCATION hdfs_path]
```

2、字段说明

1. CREATE TABLE 创建一个制定名字的表。如果相同名字的表已经存在，则抛出异常；用户可以用 IF NOT EXISTS 选项来忽略这个异常。
2. EXTERNAL 关键字可以让用户创建一个外部表，在剪标的同事指定一个指向实际数据的路径（LOCATION），Hive创建内部表时，会将数据移动到数据仓库指向的路径；若创建外部表，仅记录数据所在的路径，不对数据的位置做任何改变。在删除表的时候，内部的表元数据和数据会被一起删除，而外部表只会删除元数据，不删除数据。
3. COMMENT 为表和列添加注释
4. PARTITIONED BY 创建分区表
5. CLUSTERED BY 创建分桶表
6. SORTED BY 不常用
7. ROW FORMAT 列的分隔符
8. STORED AS 指定存储文件类型
9. LOCATION 指定表在HDFS上的存储位置
10. LIKE 允许用户复制现有的表结构，但是不复制数据

### 5.1、管理表

1、理论

​	默认创建的表都是所谓的管理表，有时也被成为内部表。因为这种表，Hive会（或多或少的）控制着数据的生命周期。Hive默认情况下会将这些表的数据存储在由配置项hive.metastore.warehouse.dir（例如 /user/hive/warehouse）所定义的目录的子目录下。当我们删除一个管理表时，Hive也会删除这个表中数据。管理表不适合和其他工具共享数据。

2、实例操作

```sh
# 1 普通创建表
create table if not exists student2(
	id int, name string
)
row format delimited fields terminated by '\t'
stored as textfile
location '/user/hive/warehouse/student2';

# 2 根据查询结果创建表（查询的结果会添加到新创建的表中）
create table if not exists student3 as select id, name from student;

# 3 根据已经存在的表结构创建表
create table if not exists student4 like student;

# 4 查询表的类型
hive (default)> desc formatted student2;
Table Type:             MANAGED_TABLE  
```

### 5.2、外部表

1、理论

​	因为表是外部表，所以Hive并非认为其完全拥有这份数据。删除该表并不会删除掉这份数据，不过描述表的元数据信息会被删除掉。

2、管理表和外部表的使用场景

​	每天将收集到的网站日志定期流入HDFS文本文件。在外部表（原始日志表）的基础商做大量的统计分析，用到的中间表，结果表使用内部表存储，数据通过SELECT+INSERT进入内部表。

3、案例实操

1）原始数据

```sh
# dept.txt
10	ACCOUNTING	1700
20	RESEARCH	1800
30	SALES	1900
40	OPERATIONS	1700
```

```sh
# emp.txt
7369	SMITH	CLERK	7902	1980-12-17	800.00		20
7499	ALLEN	SALESMAN	7698	1981-2-20	1600.00	300.00	30
7521	WARD	SALESMAN	7698	1981-2-22	1250.00	500.00	30
7566	JONES	MANAGER	7839	1981-4-2	2975.00		20
7654	MARTIN	SALESMAN	7698	1981-9-28	1250.00	1400.00	30
7698	BLAKE	MANAGER	7839	1981-5-1	2850.00		30
7782	CLARK	MANAGER	7839	1981-6-9	2450.00		10
7788	SCOTT	ANALYST	7566	1987-4-19	3000.00		20
7839	KING	PRESIDENT		1981-11-17	5000.00		10
7844	TURNER	SALESMAN	7698	1981-9-8	1500.00	0.00	30
7876	ADAMS	CLERK	7788	1987-5-23	1100.00		20
7900	JAMES	CLERK	7698	1981-12-3	950.00		30
7902	FORD	ANALYST	7566	1981-12-3	3000.00		20
7934	MILLER	CLERK	7782	1982-1-23	1300.00		10
```

2）建表语句

```sh
# 创建部门表
create external table if not exists default.dept(
    deptno int,
    dname string,
    loc int
)
row format delimited fields terminated by '\t';
```

```sh
# 创建员工表
create external table if not exists default.emp(
    empno int,
    ename string,
    job string,
    mgr int,
    hiredate string, 
    sal double, 
    comm double,
	deptno int
)
row format delimited fields terminated by '\t';
```

```sh
# 查看创建的表
hive (default)> show tables;
OK
dept
emp
```

```sh
# 向外部表中导入数据
hive (default)> load data local inpath '/opt/module/datas/dept.txt' into table default.dept;
hive (default)> load data local inpath '/opt/module/datas/emp.txt' into table default.emp;
```

```sh
# 查询结果
hive (default)> select * from emp;
hive (default)> select * from dept;
```

```sh
# 查看表格式化数据
hive (default)> desc formatted dept;
Table Type:             EXTERNAL_TABLE
```

### 5.3、管理表与外部表的相互转换

1、查询表的类型

```sh
desc formatted student2;
```

2、修改内部表student2为外部表

```sh
hive (default)> alter table student2 set tblproperties('EXTERNAL'='TRUE');
```

3、查询表的类型

```sh
hive (default)> desc formatted student2;
Table Type:             EXTERNAL_TABLE
```

4、修改外部表student2为内部表

```sh
alter table student2 set tblproperties('EXTERNAL'='FALSE');
```

5、查询表的类型

```sh
hive (default)> desc formatted student2;
Table Type:             MANAGED_TABLE
```

注意：('EXTERNAL'='TRUE') 和 ('EXTERNAL'='FALSE') 为固定写法，区分大小写！

## 6、分区表

### 6.1、分区表基本操作

​	分区表实际上就是对应一个HDFS文件系统上的独立文件夹，该文件夹下是该分区所有的数据文件。Hive中的分区就是分目录，把一个打的数据集根据业务需要分割成小的数据集。在查询时通过WHERE子句中的表达式选择查询所需要的指定的分区，这样的查询效率会提高许多。

1、引入分区表（需要根据日期对日志进行管理）

```sh
/user/hive/warehouse/log_partition/20170702/20170702.log
/user/hive/warehouse/log_partition/20170703/20170703.log
/user/hive/warehouse/log_partition/20170704/20170704.log
```

2、创建分区表语法

```sh
hive (default)> create table dept_partition(
    deptno int, 
    dname string, 
    loc string
)
partitioned by (month string)
row format delimited fields terminated by '\t';
```

3、加载数据到分区表中

```sh
hive (default)> load data local inpath '/opt/module/datas/dept.txt' into table default.dept_partition partition(month='201709');
hive (default)> load data local inpath '/opt/module/datas/dept.txt' into table default.dept_partition partition(month='201708');
hive (default)> load data local inpath '/opt/module/datas/dept.txt' into table default.dept_partition partition(month='201707’);

# 运行后可以在 /user/hive/warehouse/dept_partition/ 看到分区表
```

4、查询分区表中数据

```sh
# 单分区查询
hive (default)> select * from dept_partition where month='201709';
```

```sh
# 多分区联合查询
hive (default)> select * from dept_partition where month='201709'
              union
              select * from dept_partition where month='201708'
              union
              select * from dept_partition where month='201707';
```

5、增加分区

```sh
# 创建单个分区
hive (default)> alter table dept_partition add partition(month='201706');
```

```sh
# 同事创建多个分区
hive (default)> alter table dept_partition add partition(month='201705') partition(month='201704');
```

6、删除分区

```sh
# 删除单个分区
hive (default)> alter table dept_partition drop partition (month='201704');
```

```sh
# 删除多个分区
hive (default)> alter table dept_partition drop partition (month='201705'), partition (month='201706');
```

7、查看分区表有多少分区

```sh
hive (default)> show partitions dept_partition;
```

8、查看分区表结构

```sh
hive> desc formatted dept_partition;

# Partition Information          
# col_name              data_type               comment             
month                   string    
```

### 6.2、分区表注意事项

1、创建二级分区表

```sh
hive (default)> create table dept_partition2(
               deptno int, dname string, loc string
               )
               partitioned by (month string, day string)
               row format delimited fields terminated by '\t';
```

2、正常的加载数据

```sh
# 加载数据到二级分区表中
hive (default)> load data local inpath '/opt/module/datas/dept.txt' into table default.dept_partition2 partition(month='201709', day='13');

```

```sh
# 查询分区数据
hive (default)> select * from dept_partition2 where month = '201709' and day='13';
```

3、把数据直接上传到分区目录上，让分区表和数据产生关联的三种方式

1）方式一：上传数据后修复

```sh
# 上传数据
hive (default)> dfs -mkdir -p
 /user/hive/warehouse/dept_partition2/month=201709/day=12;
hive (default)> dfs -put /opt/module/datas/dept.txt  /user/hive/warehouse/dept_partition2/month=201709/day=12;

# 查询数据（查询不到刚上传的数据）
hive (default)> select * from dept_partition2 where month='201709' and day='12';

# 执行修复命令
hive> msck repair table dept_partition2;

# 再次查询数据
hive (default)> select * from dept_partition2 where month='201709' and day='12';
```

2）方式二：上传数据后添加分区

```sh
# 上传数据
hive (default)> dfs -mkdir -p
 /user/hive/warehouse/dept_partition2/month=201709/day=11;
hive (default)> dfs -put /opt/module/datas/dept.txt  /user/hive/warehouse/dept_partition2/month=201709/day=11;

# 执行添加分区
hive (default)> alter table dept_partition2 add partition(month='201709',
 day='11');

# 查询数据
hive (default)> select * from dept_partition2 where month='201709' and day='11';
```

3）方式三：创建文件夹后load数据到分区

```sh
#创建目录
hive (default)> dfs -mkdir -p
 /user/hive/warehouse/dept_partition2/month=201709/day=10;

# 上传数据
hive (default)> load data local inpath '/opt/module/datas/dept.txt' into table
 dept_partition2 partition(month='201709',day='10');

# 查询数据
hive (default)> select * from dept_partition2 where month='201709' and day='10';
```

## 7、修改表

### 7.1、重命名表

1、语法

```mysql
ALTER TABLE table_name RENAME TO new_table_name
```

2、实操案例

```mysql
hive (default)> alter table dept_partition2 rename to dept_partition3;
```

### 7.2、增加、修改和删除表分区

详见4.6.1分区表基本操作

### 7.3、增加、修改、替换列信息

1、语法

```mysql
# 更新列
ALTER TABLE table_name CHANGE [COLUMN] col_old_name col_new_name column_type [COMMENT col_comment] [FIRST|AFTER column_name]
```

```mysql
# 增加和替换列
ALTER TABLE table_name ADD|REPLACE COLUMNS (col_name data_type [COMMENT col_comment], ...) 

# 注：ADD是代表新增一字段，字段位置在所有列后面（partition列前），REPLACE则是表示替换表中所有字段。
```

2、实操案例

```mysql
# 1 查询表结构
hive> desc dept_partition;
```

```mysql
# 2 添加列
hive (default)> alter table dept_partition add columns(deptdesc string);
```

```mysql
# 3 更新列
hive (default)> alter table dept_partition change column deptdesc desc int;
```

```mysql
# 4 替换列
hive (default)> alter table dept_partition replace columns(deptno string, dname
 string, loc string);
```

## 8、删除表

```mysql
hive (default)> drop table dept_partition;
```

# 第五章、DML数据操作

## 1、数据导入

### 1.1、向表中装载数据（Load）

```sh
# 1 语法
hive> load data [local] inpath '/opt/module/datas/student.txt' overwrite | into table student [partition (partcol1=val1,…)];
（1）load data:表示加载数据
（2）local:表示从本地加载数据到hive表；否则从HDFS加载数据到hive表
（3）inpath:表示加载数据的路径
（4）overwrite:表示覆盖表中已有数据，否则表示追加
（5）into table:表示加载到哪张表
（6）student:表示具体的表
（7）partition:表示上传到指定分区
```

```sh
# 2 实操案例
（0）创建一张表
hive (default)> create table student(id string, name string) row format delimited fields terminated by '\t';
（1）加载本地文件到hive
hive (default)> load data local inpath '/opt/module/datas/student.txt' into table default.student;
（2）加载HDFS文件到hive中
	上传文件到HDFS
hive (default)> dfs -put /opt/module/datas/student.txt /user/atguigu/hive;
加载HDFS上数据
hive (default)> load data inpath '/user/atguigu/hive/student.txt' into table default.student;
（3）加载数据覆盖表中已有的数据
上传文件到HDFS
hive (default)> dfs -put /opt/module/datas/student.txt /user/atguigu/hive;
加载数据覆盖表中已有的数据
hive (default)> load data inpath '/user/atguigu/hive/student.txt' overwrite into table default.student;
```


### 1.2、通过查询语句向表中插入数据（Insert）

```sh
# 1 创建一张分区表
hive (default)> create table student(id int, name string) partitioned by (month string) row format delimited fields terminated by '\t';
```

```sh
# 2 基本插入数据
hive (default)> insert into table student partition(month='201709') values(1,'wangwu');
```

```sh
# 3 基本模式插入(根据单张表查询结果)
hive (default)> insert overwrite table student partition(month='201708')
                select id, name from student where month='201709';
```

```sh
# 4 多表模式插入(根据多张表查询结果)
hive (default)> from student
                insert overwrite table student partition(month='201707')
                select id, name where month='201709'
                insert overwrite table student partition(month='201706')
                select id, name where month='201709';
```


### 1.3、查询语句中创建表并加载数据（As Select）

```sh
# 根据查询结果创建表（查询的结果会添加到新创建的表中）
create table if not exists student3
as select id, name from student;
```

### 1.4、创建表时Location指定加载数据路径
```sh
# 1 创建表，并制定在hdfs上的位置
hive (default)> create table if not exists student5(
              id int, name string
              )
              row format delimited fields terminated by '\t'
              location '/user/hive/warehouse/student5';

# 2 上传数据到hdfs上
hive (default)> dfs -put /opt/module/datas/student.txt
/user/hive/warehouse/student5;

# 3 查询数据
hive (default)> select * from student5;
```

### 1.5、Import数据到指定Hive表中

```sh
# 注意：先用export导出后，再将数据导入
hive (default)> import table student2 partition(month='201709') from
 '/user/hive/warehouse/export/student';
```

## 2、数据导出

### 2.1、Insert导出

```sh
# 1 将查询的结果导出到本地
hive (default)> insert overwrite local directory '/opt/module/datas/export/student'
                select * from student;

# 2 将查询的结果格式化导出到本地
hive(default)>insert overwrite local directory '/opt/module/datas/export/student1' 
              ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' select * from student;
           
# 3 将查询的结果导出到HDFS上(没有local)
hive (default)> insert overwrite directory '/user/jay/student'
                ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' 
                select * from student;
```

### 2.2、Hadoop命令导出到本地

```sh
hive (default)> dfs -get /user/hive/warehouse/student/month=201709/000000_0
/opt/module/datas/export/student3.txt;
```

### 2.3、Hive Shell命令导出

```sh
# 基本语法：（hive -f/-e 执行语句或者脚本 > file）
[atguigu@hadoop102 hive]$ bin/hive -e 'select * from default.student;' > /opt/module/datas/export/student4.txt;
```

### 2.4、Export导出到HDFS上

```sh
hive (default)> export table default.student to
 '/user/hive/warehouse/export/student';
```

### 2.5、Sqoop导出

后续课程专门讲

## 3、清除表中数据（Truncate）

```sh
# 注意：Truncate只能删除管理表，不能删除外部表中数据
hive (default)> truncate table student;
```

# 第六章、查询

https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select

基本语句语法：

```sql
[WITH CommonTableExpression (, CommonTableExpression)*]    (Note: Only available starting with Hive 0.13.0)
SELECT [ALL | DISTINCT] select_expr, select_expr, ...
  FROM table_reference
  [WHERE where_condition]
  [GROUP BY col_list]
  [ORDER BY col_list]
  [CLUSTER BY col_list
    | [DISTRIBUTE BY col_list] [SORT BY col_list]
  ]
 [LIMIT [offset,] rows]
```

## 1、基本查询（Select...From）

### 1.1、全表和特定列查询

```mysql
# 1 全表查询
hive (default)> select * from emp;

# 2 选择特定列查询
hive (default)> select empno, ename from emp;

/* 
注意：
（1）SQL 语言大小写不敏感。 
（2）SQL 可以写在一行或者多行
（3）关键字不能被缩写也不能分行
（4）各子句一般要分行写。
（5）使用缩进提高语句的可读性。
*/
```

### 1.2、列别名

```mysql
# 查询名称和部门
hive (default)> select ename AS name, deptno dn from emp;

/*
（1）重命名一个列
（2）便于计算
（3）紧跟列名，也可以在列名和别名之间加入关键字‘AS’ 
（5）案例实操
*/
```

### 1.3、算术运算符

| 运算符 | 描述           |
| ------ | -------------- |
| A+B    | A和B 相加      |
| A-B    | A减去B         |
| A*B    | A和B 相乘      |
| A/B    | A除以B         |
| A%B    | A对B取余       |
| A&B    | A和B按位取与   |
| A\|B   | A和B按位取或   |
| A^B    | A和B按位取异或 |
| ~A     | A按位取反      |

```mysql
# 案例实操
# 查询出所有员工的薪水后加1显示。
hive (default)> select sal +1 from emp;
```

### 1.4、常用函数

```mysql
# 1 求总行数（count）
hive (default)> select count(*) cnt from emp;

# 2 求工资的最大值（max）
hive (default)> select max(sal) max_sal from emp;

# 3 求工资的最小值（min）
hive (default)> select min(sal) min_sal from emp;

# 4 求工资的总和（sum）
hive (default)> select sum(sal) sum_sal from emp; 

# 5 求工资的平均值（avg）
hive (default)> select avg(sal) avg_sal from emp;
```

### 1.5、Limit语句

```mysql
# 典型的查询会返回多行数据。LIMIT子句用于限制返回的行数。
hive (default)> select * from emp limit 5;
```

## 2、Where语句

### 2.1、比较运算符（Between/In/Is Null）

| 操作符                  | 支持的数据类型 | 描述                                                         |
| ----------------------- | -------------- | ------------------------------------------------------------ |
| A=B                     | 基本数据类型   | 如果A等于B则返回TRUE，反之返回FALSE                          |
| A<=>B                   | 基本数据类型   | 如果A和B都为NULL，则返回TRUE，其他的和等号（=）操作符的结果一致，如果任一为NULL则结果为NULL |
| A<>B, A!=B              | 基本数据类型   | A或者B为NULL则返回NULL；如果A不等于B，则返回TRUE，反之返回FALSE |
| A<B                     | 基本数据类型   | A或者B为NULL，则返回NULL；如果A小于B，则返回TRUE，反之返回FALSE |
| A<=B                    | 基本数据类型   | A或者B为NULL，则返回NULL；如果A小于等于B，则返回TRUE，反之返回FALSE |
| A>B                     | 基本数据类型   | A或者B为NULL，则返回NULL；如果A大于B，则返回TRUE，反之返回FALSE |
| A>=B                    | 基本数据类型   | A或者B为NULL，则返回NULL；如果A大于等于B，则返回TRUE，反之返回FALSE |
| A [NOT] BETWEEN B AND C | 基本数据类型   | 如果A，B或者C任一为NULL，则结果为NULL。如果A的值大于等于B而且小于或等于C，则结果为TRUE，反之为FALSE。如果使用NOT关键字则可达到相反的效果。 |
| A IS NULL               | 所有数据类型   | 如果A等于NULL，则返回TRUE，反之返回FALSE                     |
| A IS NOT NULL           | 所有数据类型   | 如果A不等于NULL，则返回TRUE，反之返回FALSE                   |
| IN(数值1, 数值2)        | 所有数据类型   | 使用 IN运算显示列表中的值                                    |
| A [NOT] LIKE B          | STRING 类型    | B是一个SQL下的简单正则表达式，如果A与其匹配的话，则返回TRUE；反之返回FALSE。B的表达式说明如下：‘x%’表示A必须以字母‘x’开头，‘%x’表示A必须以字母’x’结尾，而‘%x%’表示A包含有字母’x’,可以位于开头，结尾或者字符串中间。如果使用NOT关键字则可达到相反的效果。 |
| A RLIKE B, A REGEXP B   | STRING 类型    | B是一个正则表达式，如果A与其匹配，则返回TRUE；反之返回FALSE。匹配使用的是JDK中的正则表达式接口实现的，因为正则也依据其中的规则。例如，正则表达式必须和整个字符串A相匹配，而不是只需与其字符串匹配。 |

### 2.2、Like 和RLike

```mysql
1）使用LIKE运算选择类似的值
2）选择条件可以包含字符或数字:
	% 代表零个或多个字符(任意个字符)。
	_ 代表一个字符。
3）RLIKE子句是Hive中这个功能的一个扩展，其可以通过Java的正则表达式这个更强大的语言来指定匹配条件。
4）案例实操
	（1）查找以2开头薪水的员工信息
hive (default)> select * from emp where sal LIKE '2%';
	（2）查找第二个数值为2的薪水的员工信息
hive (default)> select * from emp where sal LIKE '_2%';
	（3）查找薪水中含有2的员工信息
hive (default)> select * from emp where sal RLIKE '[2]';
```

### 2.3、逻辑运算符（And/Or/Not）

| 操作符 | 含义   |
| ------ | ------ |
| AND    | 逻辑并 |
| OR     | 逻辑或 |
| NOT    | 逻辑否 |

## 3、分组

### 3.1、Group By 语句

```mysql
# GROUP BY语句通常会和聚合函数一起使用，按照一个或多个列队结果进行分组，然后对每个组执行聚合操作。

# 1 计算emp表每个部门的平均工资
hive (default)> select deptno, avg(sal) from emp group by deptno;

# 2 计算emp表每个岗位的最高薪水
hive (default)> select t.deptno, t.job, max(t.sal) max_sal from emp t group by
 t.deptno, t.job;
```

### 3.2、Having 语句

```mysql
# having 与 where不同点
（1）where针对表中的列发挥作用，查询数据；having针对查询结果中的列发挥作用，筛选数据。
（2）where后面不能写分组函数，而having后面可以使用分组函数。
（3）having只用于group by分组统计语句。
```

```mysql
# 案例实操
# 1 求每个部门的平均工资
hive (default)> select deptno, avg(sal) from emp group by deptno;

# 2 求每个部门的平均薪水大于2000的部门
hive (default)> select deptno, avg(sal) avg_sal from emp group by deptno having
 avg_sal > 2000;
```

## 4、Join语句

### 4.1、等值Join

```mysql
# Hive支持通常的SQL JOIN语句，但是只支持等值连接，不支持非等值连接

# 1 根据员工表和部门表中的部门编号相等，查询员工编号、员工名称和部门名称
	select e.empno, e.ename, d.dname from emp e
	join dept d on e.o = d.deptno;
```

### 4.2、表的别名

```
（1）使用别名可以简化查询。
（2）使用表名前缀可以提高执行效率。
```

### 4.3、内连接

```mysql
# 只有进行连接的两个表中都存在与连接条件相匹配的数据才会被保留下来
select e.* from emp e join dept d on e.deptno = d.deptno;
```

### 4.4、左外连接

```mysql
# Join操作符左边表中符合where子句的所有记录将被返回
select e.* from emp e left join dept d on e.deptno = d.deptno;
```

### 4.5、右外连接

```mysql
# Join操作符右边表中符合where子句的所有记录将被返回
select e.* from emp e right join dept d on e.deptno = d.deptno;
```

### 4.6、满外连接

```mysql
# 将会返回所有表中符合where语句条件的所有记录。如果任一表的指定字段没有符合条件的值的画，那么就使用NULL值替代
select e.* from emp e full join dept d on e.deptno = d.deptno;
```

### 4.7、多表连接

```mysql
# 1 创建位置表
create table if not exists default.location(
    loc int,
    loc_name string
)
row format delimited fields terminated by '\t';
```

```mysql
# 2 导入数据
hive (default)> load data local inpath '/opt/module/datas/location.txt' into table default.location;
```

```mysql
# 3 多表连接查询
hive (default)>SELECT e.ename, d.deptno, l. loc_name
FROM   emp e 
JOIN   dept d ON     d.deptno = e.deptno 
JOIN   location l ON     d.loc = l.loc;

# 大多数情况下，Hive会对每个JOIN连接对象启动一个MapReduce任务。本例中会首先启动一个MapReduceJob对 表e 和 表d 进行连接操作，然后会再启动一个MapReduceJob将第一个MapReduceJob的输出和 表l 进行连接操作。
# 注意：为什么不是 表d 和 表l 先进行连接操作呢？这时因为Hive总是按照从左到右的顺序执行的。
```

### 4.8、笛卡尔积

1、笛卡尔集合会在下面条件下产生

1. 省略连接条件
2. 连接条件无效
3. 所有表中的所有行互相连接

```mysql
# 案例实操
hive (default)> select empno, dname from emp, dept;
```

### 4.9、连接谓词中不支持or

```mysql
hive (default)> select e.empno, e.ename, d.deptno from emp e join dept d on e.deptno
= d.deptno or e.ename=d.ename;
# 错误的
```

## 5、排序

### 1、全局排序（Order By）

Order By：全局排序，一个Reducer

1、使用Order By子句排序

ASC（ascend）：升序（默认）

DESC（descend）：降序

2、Order By 子句在select 语句的结尾

3、实操案例

```mysql
# 1 查询员工信息按工资升序排列
hive (default)> select * from emp order by sal;

# 2 查询员工信息按工资降序排列
hive (default)> select * from emp order by sal desc;
```

### 2、按照别名排序

```mysql
# 按照员工薪水的2倍排序
hive (default)> select ename, sal*2 twosal from emp order by twosal;
```

### 3、多个列排序

```mysql
# 按照部门和工资生序排序
hive (default)> select ename, deptno, sal from emp order by deptno, sal;
```

### 4、每个MapReduce内部排序（Sort By）

```mysql
# Sort By:每个Reducer内部进行排序，对全局结果集来说不是排序




```



### 5、



### 6、



### 7、



### 8、



### 9、

## 6、分桶及抽样查询

### 1、二级目录



### 2、



### 3、



### 4、



### 5、



### 6、



### 7、



### 8、



### 9、

## 7、其他常用查询函数

### 1、二级目录



### 2、



### 3、



### 4、



### 5、



### 6、



### 7、



### 8、



### 9、

# 第七章、函数

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

6）

7）

8）

9）



## 2、一级目录

### 1、二级目录



### 2、



### 3、



### 4、



### 5、



### 6、



### 7、



### 8、



### 9、



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




