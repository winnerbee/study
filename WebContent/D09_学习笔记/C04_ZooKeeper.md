# 大数据之ZooKeeper技术

# 第一章、Zookeeper入门

## 1、概述

ZooKeeper是一个开源的分布式的，为分布式应用提供协调服务的Apache项目。

## 2、特点

1、ZooKeeper：一个领导者（Leader），多个跟随者（Follower）组成的集群。

2、集群中只要有半数以上节点存活，ZooKeeper集群就能正常服务。

3、全局数据一致：每个server保存一份相同的数据副本，client无论连接到哪个server，数据都是一致的。

4、更新请求顺序进行，来自同一个client的更新请求按其发送顺序一次执行。

5、数据更新原子性，一次数据要么成功，要么失败。

6、实时性，在一定时间范围内，client能读到最新的数据。

## 3、数据结构

ZooKeeper数据结构与Unix文件系统很类似，整体上可以看作是一棵树，每个节点称作一个ZNode。每一个ZNode默认能够存储1MB的数据，每个ZNode都可以通过其路径唯一标示。

## 4、应用场景

提供的服务包括：统一命名服务、统一配置管理、统一集群管理、服务器节点动态上下线、软负载均衡等。

统一命名服务：在分布式环境下，经常需要对应用、服务器进行统一命名，便于识别。例如：IP不容易记住，而域名更容易记住。

统一配置管理：

1. 分布式环境下，配置文件同步非常常见。

   1）一般要求一个集群中所有的配置信息是一致的，比如Kafka集群。

   2）对配置文件修改后，希望能够快速同步到各个节点上。

2. 配置管理可交由ZooKeeper实现。

   1）可将配置信息写入ZooKeeper商的一个Znode。

   2）各个客户端服务器监听这个Znode。

   3）一旦Znode中的数据被修改，ZooKeeper将通知各个客户端服务器。

统一集群管理：

1. 分布式环境中，实施掌握每个节点的状态是必须的。

   1）可根据节点实时状态做出一些调整。

2. ZooKeeper可以实现

3. 实时监控节点状态变化

   1）可将节点信息写入ZooKeeper上的一个Znode。

   2）监听这个ZNode可获取它的实时状态变化。

服务器动态上下线：

​	服务器能实时洞察到服务器上下线的变化。

软负载均衡：

​	在ZooKeeper中记录每台服务器的访问数，让访问数最少的服务器去处理最新的客户端请求。

## 5、下载地址

https://zookeeper.apache.org

# 第二章、ZooKeeper安装

## 1、本地模式安装部署

1）安装jdk

2）拷贝ZooKeeper安装包到Linux系统下

3）解压到指定目录

```sh
tar -zxvf zookeeper-3.4.10.tar.gz -C /opt/module/
```

## 2、常用命令

```sh
# 启动服务器
bin/zkServer.sh start

# 停止服务器
bin/zkServer.sh stop

# 查看进程是否启动
jps

# 查看状态
bin/zkServer.sh status

# 启动客户端
bin/zkCli.sh

# 退出客户端
quit

# 查看当前znode中所包含的内容
ls /

# 查看当前节点详细数据
ls2 /

# 分别创建2个普通节点
create /sanguo "jinlian"
create /sanguo/shuguo "liubei"

# 获得节点的值
get /sanguo

# 创建短暂节点
create -e /sanguo/wuguo "zhouyu"
（1）在当前客户端是能查看到的
[zk: localhost:2181(CONNECTED) 3] ls /sanguo 
[wuguo, shuguo]
（2）退出当前客户端然后再重启客户端
[zk: localhost:2181(CONNECTED) 12] quit
[atguigu@hadoop104 zookeeper-3.4.10]$ bin/zkCli.sh
（3）再次查看根目录下短暂节点已经删除
[zk: localhost:2181(CONNECTED) 0] ls /sanguo
[shuguo]

# 创建带序号的节点
（1）先创建一个普通的根节点/sanguo/weiguo
[zk: localhost:2181(CONNECTED) 1] create /sanguo/weiguo "caocao"
Created /sanguo/weiguo
（2）创建带序号的节点
[zk: localhost:2181(CONNECTED) 2] create -s /sanguo/weiguo/xiaoqiao "jinlian"
Created /sanguo/weiguo/xiaoqiao0000000000
[zk: localhost:2181(CONNECTED) 3] create -s /sanguo/weiguo/daqiao "jinlian"
Created /sanguo/weiguo/daqiao0000000001
[zk: localhost:2181(CONNECTED) 4] create -s /sanguo/weiguo/diaocan "jinlian"
Created /sanguo/weiguo/diaocan0000000002
如果原来没有序号节点，序号从0开始依次递增。如果原节点下已有2个节点，则再排序时从2开始，以此类推。

# 修改节点数据值
[zk: localhost:2181(CONNECTED) 6] set /sanguo/weiguo "simayi"

#节点的值变化监听
（1）在hadoop104主机上注册监听/sanguo节点数据变化
[zk: localhost:2181(CONNECTED) 26] [zk: localhost:2181(CONNECTED) 8] get /sanguo watch
（2）在hadoop103主机上修改/sanguo节点的数据
[zk: localhost:2181(CONNECTED) 1] set /sanguo "xisi"
（3）观察hadoop104主机收到数据变化的监听
WATCHER::
WatchedEvent state:SyncConnected type:NodeDataChanged path:/sanguo


# 节点的子节点变化监听（路径变化）
（1）在hadoop104主机上注册监听/sanguo节点的子节点变化
[zk: localhost:2181(CONNECTED) 1] ls /sanguo watch
[aa0000000001, server101]
（2）在hadoop103主机/sanguo节点上创建子节点
[zk: localhost:2181(CONNECTED) 2] create /sanguo/jin "simayi"
Created /sanguo/jin
（3）观察hadoop104主机收到子节点变化的监听
WATCHER::
WatchedEvent state:SyncConnected type:NodeChildrenChanged path:/sanguo

# 删除节点
[zk: localhost:2181(CONNECTED) 4] delete /sanguo/jin

# 递归删除节点
[zk: localhost:2181(CONNECTED) 15] rmr /sanguo/shuguo

# 查看节点状态
[zk: localhost:2181(CONNECTED) 17] stat /sanguo
cZxid = 0x100000003
ctime = Wed Aug 29 00:03:23 CST 2018
mZxid = 0x100000011
mtime = Wed Aug 29 00:21:23 CST 2018
pZxid = 0x100000014
cversion = 9
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 1
```

## 3、配置参数解读

ZooKeeper中的配置文件zoo.cfg中参数含义解读如下：

```properties
# The number of milliseconds of each tick
# 通信心跳数，ZooKeeper服务器与客户端心跳时间，单位毫秒
# 它用于心跳机制，并且设置最小的session超时时间为两倍心跳时间（session的最小超时时间是 2* tickTime）
tickTime=2000

# The number of ticks that the initial 
# synchronization phase can take
# LF初始通信时限
# 急群众的Follower跟随者服务器与Leader领导者服务器之间初始连接时能容忍的最多心跳数（tickTime的数量），用它来限定集群中的ZooKeeper服务器连接到Leader的时限。
initLimit=10

# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
# LF同步通信时限
# 急群众Leader与Follower之间的最大响应时间单位，假如超过 syncLimit * tickTime时限后，Leader认为Follwer死掉，从服务器表中删除Follwer。
syncLimit=5

# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
# 数据文件目录+数据持久化路径
dataDir=/opt/module/zookeeper-3.4.10/zkData

# the port at which the clients will connect
# 客户端连接端口
clientPort=2181

# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
#######################cluster##########################
# server.A=B:C:D
# A是一个数字，表示这个是第几号服务器
# 集群模式下配置一个文件myid，这个文件在dataDir目录下，这个文件里面由一个数据就是A的值，ZooKeeper启动时读取此文件，拿到里面的数据与zoo.cfg里面的配置信息比较从而判断到底哪个是server
# B 是这个服务器的ip地址
# C 是这个服务器与集群中的Leader服务器交换信息的端口
# D 是万一集群中的Leader服务器挂了，需要一个端口来重新进行选举，选出一个新的Leader，而这个端口就是用来执行选举时服务器相互通信的端口
server.1=hadoop101:2888:3888
server.2=hadoop102:2888:3888
server.3=hadoop103:2888:3888
```

# 第三章、ZooKeeper内部原理

## 3.1、选举机制（面试重点）

1）半数机制：集群中半数以上机器存活，集群可用。所以ZooKeeper适合安装奇数台服务器。

2）ZooKeeper虽然在配置文件中并没有指定Master和Slave。但是，ZooKeeper工作时，是由一个节点为Leader，其他则为Follower，Leader是通过内部的选举机制临时产生的。

3）以一个简单的例子来说明整个选举的过程。

​	假设由五台服务器组成的ZooKeeper集群，它们的id是1-5，同时他们都是最新启动的，也就是没有历史数据，在存放数据量这一点上，都是一样的。假设这些服务器依序启动，来看看会发生什么。

![选举机制](大数据md图片\选举机制.png)

1）服务器1启动，此时只有它一台服务器启动着，它发出的报文没有任何响应，所以它的选举状态一直是LOOKING状态。

2）服务器2启动，它与最开始启动的服务器1进行通信，互相交换自己的选举结果，由于两者都没有历史数据，所以id值较大的服务器2胜出，但是由于没有达到超过半数以上的服务器都同意选举它（这个例子中的半数以上是3），所以服务器1、2还是继续保持LOOKING状态

3）服务器3启动，根据前面的理论分析，服务器3成为服务器1、2、3中的老大，而与上面不同的是，此时由三台服务器选举了它，所以它成为了这次选举的Leader。

4）服务器4启动，根据前面的分析，理论上服务器4应该是服务器1、2、3、4中最大的，但是由于前面已经有半数以上的服务器选举了服务器3，所以它只能接收当小弟的命了。

5）服务器5启动，同4一样当小弟。

## 3.2、节点类型

持久（Persistent）：客户端和服务器断开连接后，创建的节点不删除

短暂（Ephemeral）：客户端和服务器断开连接后，创建的节点自己删除

1、持久目录节点

​	客户端与ZooKeeper断开连接后，该节点依旧存在

2、持久化顺序编号目录节点

​	客户端与ZooKeeper断开连接后，该节点依旧存在，只是ZooKeeper给该节点名称进行顺序编号

3、临时目录节点

​	客户端与ZooKeeper断开连接后，该节点被删除

4、临时顺序编号目录节点

​	客户端与ZooKeeper断开连接后，该节点被删除，只是ZooKeeper给该节点名称进行顺序编号

说明：创建znode时设置顺序标识，znode名称后会附加一个值，顺序号是一个单调递增的计数器，由父节点维护。

注意：在分布式系统中，顺序号可以被用于为所有的事件进行全局排序，这样客户端可以通过顺序号推断事件的顺序。

## 3.3、Stat结构体

意义不大，开发中不用

## 3.4、监听器原理（面试重点）

1. 首先要有一个main()线程
2. 在main线程中创建ZooKeeper客户端，这时就会创建两个线程，一个负责网络连接通信(connet)，一个负责监听(listener)。
3. 通过connect线程将注册的监听事件发送给ZooKeeper。
4. 在ZooKeeper的注册监听器列表中将注册的监听事件添加到列表中。
5. ZooKeeper监听到有数据或路径变化，就会将这个消息发送给listener线程。
6. listener线程内部调用了process()方法。

![监听器原理](大数据md图片\监听器原理.jpg)

## 3.5、写数据流程

1. Client向ZooKeeper的Server1上写数据，发送一个写请求。
2. 如果Server1不是Leader，那么Server1会把接受到的请求进一步转发给Leader，因为每个ZooKeeper的Server里面由一个是Leader。这个Leader会将写请求广播给各个Server，比如Server1和Server2，各个Server写成功后就会通知Leader。
3. 当Leader收到大多数Server数据写成功了，那么就说明数据写成功了。如果这里三个节点的话，只要由两个节点数据写成功了，那么就认为数据写成功了。写成功之后，Leader会告诉Server1数据写成功了。
4. Server1会进一步通知Client数据写成功了，这时就认为整个写操作成功。

![写数据流程](大数据md图片\写数据流程.jpg)

# 第四章、ZooKeeper实战（开发重点）

## 1、分布式安装部署

## 2、客户端命令行操作

## 3、Eclipse开发环境-API应用

1、创建maven工程

2、pom文件

```xml
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
<!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.10</version>
</dependency>
```

3、log4j文件

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

4、java测试类

```java
public class TestZooKeeper {
	
	private String connectString = "hadoop101:2181,hadoop102:2181,hadoop103:2181";	// 连接集群
	private int sessionTimeout = 2000;	// 超时时间 毫秒
	private ZooKeeper zkClient;

	@Before
	public void init() throws Exception {
		
		zkClient = new ZooKeeper(connectString, sessionTimeout, new Watcher() {
			
			@Override
			public void process(WatchedEvent event) {
				System.out.println("-----------start-------------");
				try {
					String path = "/";
					boolean watch = true;
					List<String> children = zkClient.getChildren(path, watch);
					for (String string : children) {
						System.out.println(string);
					}
				} catch (Exception e) {
					e.printStackTrace();
				} 
			}
		});
	}
	
	// 1 创建节点
	@Test
	public void createNode() throws Exception {
		String path = "/atguigu";
		byte[] data = "哈哈哈哈".getBytes();
		List<ACL> acl = Ids.OPEN_ACL_UNSAFE;
		CreateMode createMode = CreateMode.PERSISTENT;
		String create = zkClient.create(path, data, acl, createMode);
		System.out.println(create);
	}
	
	// 2 获取子节点并监控节点的变化
	@Test
	public void getDataAndWatch() throws Exception {
		Thread.sleep(Long.MAX_VALUE);
	}
	
	// 3 判断节点是否存在
	@Test
	public void exist() throws KeeperException, InterruptedException {
		String path = "/atguigu";
		boolean watch = false;
		Stat exists = zkClient.exists(path, watch);
		System.out.println(exists == null ? "not exist" : "exist");
	}
}
```

## 4、监听服务器节点动态上下线



# 第五章、企业真实面试题（面试重点）



第三章 

学习进度 20/25

16~20  算 5课

今天 必须结束，后面还要学其他的