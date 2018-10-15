# Zookeeper集群

Zookeeper连接信息: 
10.19.53.222:2181,10.19.91.16:2181,10.19.36.184:2181

## Zookeeper集群环境部署

### 1. 服务器环境

|IP            |ID   |Version           |`ZK_HOME`
|:-------------|:----|:-----------------|:------
|10.19.53.222  |1    |zookeeper-3.4.13  |/opt/app/zookeeper/zookeeper-3.4.13
|10.19.91.16   |2    |zookeeper-3.4.13  |/opt/app/zookeeper/zookeeper-3.4.13
|10.19.36.184  |3    |zookeeper-3.4.13  |/opt/app/zookeeper/zookeeper-3.4.13

### 2. Zookeeper基本配置

创建`$ZK_HOME/data`和`$ZK_HOME/datalog`
    
    mkdir /opt/app/zookeeper/zookeeper-3.4.13/{data,datalog}
    
修改zoo.cfg配置文件`$ZK_HOME/conf/zoo.cfg`, 使用vi或vim打开zoo.cfg, 增加或修改如下配置

```
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/opt/app/zookeeper/zookeeper-3.4.13/data
dataLogDir=/opt/app/zookeeper/zookeeper-3.4.13/datalog
clientPort=2181
maxClientCnxns=60
autopurge.snapRetainCount=3
autopurge.purgeInterval=1
server.1=10.19.53.222:2288:3388
server.2=10.19.91.16:2288:3388
server.3=10.19.36.184:2288:3388
```

server.1对应主机10.19.53.222，ID为1，server.2对应主机为10.19.91.16，ID为2，server.3对应主机为10.19.36.184，ID为3

根据zookeeper集群角色ID定义每个主机节点的`myid`，用于`ZK`集群主机节点的ID标识，以下命令分别在对应主机节点上执行

```bash
$ echo 1 > /opt/app/zookeeper/zookeeper-3.4.13/data/myid  #Node1
$ echo 2 > /opt/app/zookeeper/zookeeper-3.4.13/data/myid  #Node2
$ echo 3 > /opt/app/zookeeper/zookeeper-3.4.13/data/myid  #Node3
```
### 3. Zookeeper集群启动

在每个节点定义好`server.id`，并完成zookeeper集群的基本配置之后，接下来启动`Zookeeper`集群

分别在每个节点使用`zkServer.sh`脚本启动zk服务

```bash
$ cd /opt/app/zookeeper/zookeeper-3.4.13/bin
$ ./zkServer.sh start
```
## Zookeeper集群连接测试

在任意节点使用`zkCli.sh`脚本连接zk服务集群

```bash
./zkCli.sh -server 10.19.53.222:2181,10.19.91.16:2181,10.19.36.184:2181
```

### 连接示例

```
# zkCli.sh -server 10.19.53.222:2181,10.19.91.16:2181,10.19.36.184:2181
Connecting to 10.19.53.222:2181,10.19.91.16:2181,10.19.36.184:2181
2018-10-09 14:27:50,141 [myid:] - INFO  [main:Environment@100] - Client environment:zookeeper.version=3.4.13-2d71af4dbe22557fda74f9a9b4309b15a7487f03, built on 06/29/2018 04:05 GMT
2018-10-09 14:27:50,144 [myid:] - INFO  [main:Environment@100] - Client environment:host.name=localhost
2018-10-09 14:27:50,145 [myid:] - INFO  [main:Environment@100] - Client environment:java.version=1.8.0_181
2018-10-09 14:27:50,146 [myid:] - INFO  [main:Environment@100] - Client environment:java.vendor=Oracle Corporation
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:java.home=/usr/local/jdk1.8.0_181/jre
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:java.class.path=/opt/app/zookeeper/zookeeper-3.4.13/bin/../build/classes:/opt/app/zookeeper/zookeeper-3.4.13/bin/../build/lib/*.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../lib/slf4j-log4j12-1.7.25.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../lib/slf4j-api-1.7.25.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../lib/netty-3.10.6.Final.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../lib/log4j-1.2.17.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../lib/jline-0.9.94.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../lib/audience-annotations-0.5.0.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../zookeeper-3.4.13.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../src/java/lib/*.jar:/opt/app/zookeeper/zookeeper-3.4.13/bin/../conf:/usr/local/jdk/lib:/usr/local/jdk/jre/lib
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:java.library.path=/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:java.compiler=<NA>
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:os.name=Linux
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:os.arch=amd64
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:os.version=3.10.0-862.9.1.el7.x86_64
2018-10-09 14:27:50,147 [myid:] - INFO  [main:Environment@100] - Client environment:user.name=root
2018-10-09 14:27:50,148 [myid:] - INFO  [main:Environment@100] - Client environment:user.home=/root
2018-10-09 14:27:50,148 [myid:] - INFO  [main:Environment@100] - Client environment:user.dir=/root
2018-10-09 14:27:50,149 [myid:] - INFO  [main:ZooKeeper@442] - Initiating client connection, connectString=10.19.53.222:2181,10.19.91.16:2181,10.19.36.184:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@277050dc
Welcome to ZooKeeper!
2018-10-09 14:27:50,169 [myid:] - INFO  [main-SendThread(10-19-36-184:2181):ClientCnxn$SendThread@1029] - Opening socket connection to server 10-19-36-184/10.19.36.184:2181. Will not attempt to authenticate using SASL (unknown error)
JLine support is enabled
2018-10-09 14:27:50,227 [myid:] - INFO  [main-SendThread(10-19-36-184:2181):ClientCnxn$SendThread@879] - Socket connection established to 10-19-36-184/10.19.36.184:2181, initiating session
2018-10-09 14:27:50,240 [myid:] - INFO  [main-SendThread(10-19-36-184:2181):ClientCnxn$SendThread@1303] - Session establishment complete on server 10-19-36-184/10.19.36.184:2181, sessionid = 0x30032581ad50005, negotiated timeout = 30000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
[zk: 10.19.53.222:2181,10.19.91.16:2181,10.19.36.184:2181(CONNECTED) 0]
```

