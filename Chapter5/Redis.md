# redis




## 一、redis 前世今生

### author: 

出生于西西里岛的意大利人*Salvatore* *Sanfilippo*（antirez）。个人网站：http://invece.org

### 诞生背景：

antirez早年是系统管理员，2004到2006年做嵌入式工作，之后接触web，2007年和朋友共同创建一个网站LLOOGG.com,并为了解决这个网站的负载问题，**而在2009年开发了Redis数据库**。

 ### LLOOGG.com网站

#### 网站简介

LLOOGG.com网站是一个访客信息网站，网站可以通过javascript脚步，将访客IP地址、所属的国家、阅览信息、访问网页地址传送给LLOOGG.com网站。

#### 网站架构

三个网站同时向LLOOGG.com发送它们的访客浏览记录，然后LLOOGG.com会将这些浏览数据通过web页面实时展示给用户，并存储最新的5到10000条浏览记录，以便进行查阅，就是说可以设置查看最近多少条。

Google的Analytics直到2011年才有了实时功能，所以说LLOOGG.com实时反馈想法在当时2007年还是很有新意的。

#### 网站运作方式

这里有三个网站，为了记录每个追踪网站的浏览信息，LLOOGG.com需要为每个追踪的网站创建一个列表（list），每个列表需要根据用户的设置，存储最新的5到10000条浏览记录。

各个网站发送的浏览记录会分别进入相应的队列。



## 二、redis 资料

### 官方文档

####  指令

https://www.redis.net.cn/order/3667.html

[指定练习](https://try.redis.io/)



## 三、redis 安装& 配置

### 安装（linux）

https://redis.io/download

https://www.redis.net.cn/download

####  从源代码

使用以下命令下载，提取和编译Redis：

```
$ wget https://download.redis.io/releases/redis-6.0.10.tar.gz
$ tar xzf redis-6.0.10.tar.gz
$ cd redis-6.0.10
$ make
```

`meke test` 命令后出现  【ok】xxx 说明make成功。

`src` 目录 中现在提供了已编译的二进制文件 。使用以下命令运行Redis：

```
$ cd src
./redis-server ../redis.conf
```





#####   保护模式去除

不去除只有 redis所在主机才能连接。其它则报

```
DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
```

#####   解决

**修改redis.conf 文件。启动**

redis_home下 vi redis.conf ；在redis_home 下的src目录 有  redis-server。redis-cli等。

* 1、bind 注释掉  2protected-mode no

```
################################## NETWORK #####################################
#bind 127.0.0.1 
```



* 启动命令 src目录下

`./redis-srver ../redis.conf`





#####  一些问题

######  P1 GCC版本较低问题

```
server.c:5491:15: error: ‘struct redisServer’ has no member named ‘maxmemory’
     if (server.maxmemory > 0 && server.maxmemory < 1024*1024) {
               ^
server.c:5491:39: error: ‘struct redisServer’ has no member named ‘maxmemory’
     if (server.maxmemory > 0 && server.maxmemory < 1024*1024) {
                                       ^
server.c:5492:176: error: ‘struct redisServer’ has no member named ‘maxmemory’
         serverLog(LL_WARNING,"WARNING: You specified a maxmemory value that is less than 1MB (current value is %llu bytes). Are you sure this is what you really want?", server.maxmemory);
                                                                                                                                                                                ^
server.c:5495:31: error: ‘struct redisServer’ has no member named ‘server_cpulist’
     redisSetCpuAffinity(server.server_cpulist);
                               ^
server.c: In function ‘hasActiveChildProcess’:
```



```
yum -y install centos-release-scl

yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils

scl enable devtoolset-9 bash

echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile

yum install tcl
```



######   P2： make test 有错误

```
 "test_client_main $::test_server_port "
Killing still running Redis server 30497
Killing still running Redis server 30568
Killing still running Redis server 30628
Killing still running Redis server 30845
Killing still running Redis server 358
Killing still running Redis server 1327
Killing still running Redis server 1447
Killing still running Redis server 3712
make[1]: *** [test] Error 1
make[1]: Leaving directory `/usr/local/redis/src'
make: *** [test] Error 2
```

**A:**  忽略。

  **待验证有效性**

vim redis_home/tests/integration/replication-psync.tcl

把after 后面的值设置为1000,然后重新make test



*make install PREFIX=/usr/local/redis*



### 配置

#### 基础配置

`config get  <parameter>` 

例子： config get *max-*-entries* 

#### 持久化配置

**在redis.conf里配置。**

expire  a  120 。save。 redis服务关闭。然后120s后启动。ttl a : -2。

根据命令记录时间、设置多少s后过期和当前时间来就算ttl的。

##### RDB 

* **开启关闭 （默认开启）**
  * 开启：save <seconds> <changes>存在
  * 关闭：save <seconds> <changes> 语句都删除掉/注释掉。或者 save ""

* **指定本地数据库文件名**，一般采用默认的 dump.rdb

  `dbfilename dump.rdb`  **不可以写路径**，路径不可变；只能改名称

* **文件所在目录：** dir <path>

  例：默认`./`  在redis_home下。和redis.conf同一级。真实路径和相对路径均可，单必须保证文件夹存在

  windows 下

  ​			真实路径最后不加\。例：`f:\0`

  ​			相对路径加不加\都行。

  linux 下：//todo

* **rdb文件校验**  `rdbchecksum yes`  多耗费10%性能

* **bgsave发生错误时是否停止写入**

  `stop-writes-on-bgsave-error yes`

* 

  

* **压缩**  

  默认开启lzf数据压缩  `rdbcompression yes`         默认yes

* **rdb数据校验：**  `rdbchecksum yes`       默认yes

* **快照方案**

  save <seconds> <changes>

  ```
  #   In the example below the behaviour will be to save:
  #   after 900 sec (15 min) if at least 1 key changed
  #   after 300 sec (5 min) if at least 10 keys changed
  #   after 60 sec if at least 10000 keys changed
  save 900 1
  save 300 10
  save 60 10000
  ```

  0s启动。

  900 s 内有1次key 改变，就保存。但不是立即保存。而是上一个save 完成后。

  若900s的前300s内，有10次改变。会在第300s保存。

  然后从第300s后开始（即此时的第300内相当于 启动时候的0s ）。

  

  若0-900s内都没有命令执行，到1000s才有命令执行。那么在第1000s命令执行完成后，会立即save。

  **changed改变：**

  * **exprie 、set、del 同一个或不同一个key叫改变；**

  * **ttl、get key不属于改变，错误命令没有执行直接抛错也不叫改变）**

  例：

  save 120 1
  save 60  3

  ```
  [5904] 07 Feb 15:36:35.207 # Server started, Redis version 3.0.504
  [5904] 07 Feb 15:36:35.208 * DB loaded from disk: 0.000 seconds
  [5904] 07 Feb 15:36:35.208 * The server is now ready to accept connections on port 6379
  [5904] 07 Feb 15:37:36.000 * 3 changes in 60 seconds. Saving...
  [5904] 07 Feb 15:37:36.006 * Background saving started by pid 17180
  [5904] 07 Feb 15:37:36.107 # fork operation complete
  [5904] 07 Feb 15:37:36.107 * Background saving terminated with success
  [5904] 07 Feb 15:38:37.013 * 3 changes in 60 seconds. Saving...
  [5904] 07 Feb 15:38:37.019 * Background saving started by pid 13456
  [5904] 07 Feb 15:38:37.119 # fork operation complete
  [5904] 07 Feb 15:38:37.119 * Background saving terminated with success
  ```

  



##### AOF 

​	主机文件保存基础知识sync：输入字符/命令 写入文件的过程。字符/命令 现在内存/缓存中保存，等一定规则后（缓存达到一定值，一定时间，os配置策略达成）才会把缓存内容写到主机磁盘里。

- **开启关闭 （默认关启）**
  - 开启：`appendonly yes`
  - 关闭：`appendonly no` 

- **aof文件名**，一般采用默认的 appendonly.aof

  `appendfilename "appendonly.aof"`  **不可以写路径**，路径不可变；只能改名称

- **文件所在目录：** dir <path>  和rdb的dir是一个。

  例：默认`./`  在redis_home下。和redis.conf同一级。真实路径和相对路径均可，单必须保证文件夹存在

  windows 下

  ​			真实路径最后不加\。例：`f:\0`

  ​			相对路径加不加\都行。

  linux 下：//todo

- **AOF持久化策略**

  * appendfsync alwagys     同步持久化，每次发生数据变化会立刻写入到磁盘中
  * appendfsync everysec    默认 ，每秒写入到磁盘中
  * appendfsync no             redis不强制用命令同步写入磁盘，**但OS会，根据OS默认的配置**
  
  

* **no-appendfsync-on-rewrite**  重写的时候不进行appendfsync（即刷盘）。

  * no 默认=重写的时候进行刷盘，

     但是安全，命令无遗漏（宕机情况下），保证了数据一致性； 若文件过大，客户端会有阻塞。

  * yes 重写的时候不进行刷盘，

     不安全，命令可能会遗漏（宕机情况下），不能保证数据一致性；但是客户端感觉较好。

    

  

* **aof重写规则配置**
  
```
  auto-aof-rewrite-percentage 100
  auto-aof-rewrite-min-size 64mb
```

  当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发

  **关闭重写：**  auto-aof-rewrite-percentage 0

  

## 四、redis 结构&命令







## 五、redis 持久化

### 基本常识



### rdb、aof比较

* aof优先级较高，若配置了aof，恢复使用aof





### RDB（Redis DataBase） 快照

#### 原理&方案

 备份时占用内存，因为Redis 在备份时会独立创建一个子进程，将数据写入到一个临时文件（此时内存中的数据是原来的两倍哦），最后再将临时文件替换之前的备份文件。



#### 触发RDB操作

- 快照方案 changed执行
- save
- bgsave
- flushall
- shutdown 

#### 通过RDB 恢复数据 
若配置的rdb文件没损坏，则重启redis服务即可。若损坏，把备份文件copy过来，重启服务。





#### 优点

* 保存整体占用空间小

* 恢复速度快



#### 缺点

* 最后一次保存到宕机之间的操作无法保存。





#### 适用场景



***



### AOF（Append Only File）

redis 速度快： 核心是基于**非阻塞的 IO 多路复用机制**  [链接](https://www.cnblogs.com/yanguhung/p/10145755.html)

#### 原理

redis命令日志 保存到内存再保存到aof文件。

####  aof重写

##### 定义&操作

`AOF`重写是`AOF`持久化的一个机制，用来压缩AOF文件；

1. 主进程fork一个子进程，进行重写操作

2. 第二步，子进程生成redis内存数据快照（计算内存中命令叠加后的结果）保存到 AOF重写缓冲区。

3. 第三步，AOF重写缓冲区的内容写入主机磁盘（通过OS的sync命令），生成新的AOF文件，

4. 第四步，再把新的文件改名为原来的AOF文件名，用来覆盖到原来的AOF文件



#### 重写和刷盘 优先级方案：

**no-appendfsync-on-rewrite**  重写的时候不进行刷盘（appendfsync）。

##### no-appendfsync-on-rewrite no 默认，**重写的时候进行刷盘，重写和刷盘优先级一样**；

此时刷盘操作appendsync和重写操作rewrite的第二步（计算的redis内存结果从重写缓冲区到磁盘）竞争OS的 sync资源，造成主进程阻塞（客户端执行的命令）。 但是安全，命令无遗漏，保证了数据一致性。

即使竞争失败且重写进程执行中宕机，但主进程并没有返回客户端命令成功，而是等待中，数据还是一致的。

**优缺点：**安全，命令无遗漏（宕机情况下），保证了数据一致性**；**    若文件过大，客户端会有阻塞。

**流程图：**



//todo



appendfsync always

no-appendfsync-on-rewrite no 

| 时间 | redis主进程                                                  | redis子进程                                                  |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| T1   | set k1 v1   ->进入缓冲 ->主进程计算->反馈客户端  ；调用另一个子进程->申请执行sync命令->缓冲命令 刷盘成功 |                                                              |
| T2   | set k2 v2     ->进入缓冲 ->主进程计算->反馈客户端  ；调用另一个子进程->申请执行sync命令->缓冲命令 刷盘成功 | 满足重写规则，开始重写 //todo  检测是否满足重写规则，是主进程还是另外的子进程 |
| T3   | 创建AOF重写子进程                                            | 开始重写操作，生成快照/开始计算redis内存结果（T1,T2 命令结果） |
| T4   | set k3 v3    ->进入缓冲 ->主进程计算->反馈客户端  ；调用另一个子进程->申请执行sync命令->缓冲命令 刷盘成功 | 重写操作执行中/计算redis内存结果中                           |
| T5   | set k4 v4  ->进入缓冲 ->主进程计算->反馈客户端  ；调用另一个子进程->申请执行sync命令-> | 计算完毕，结果快照在AOF重写缓冲区。申请 OS sync              |
| T52  | -> wait                                                      | **若子进程抢占到sync资源**，写入到新aof文件                  |
| T6   | -> wait                                                      | 写入到新aof文件完毕。                                        |
| T7   | -> wait                                                      | 改名新aof文件名和老aof文件名一样->覆盖老文件->发送信号到主进程 |
| T8   | 接收到信号，主进程此时有空闲，执行T5-T7时刻的命令            |                                                              |



T52-T7时刻，用户等待

##### no-appendfsync-on-rewrite yes **重写的时候不进行刷盘，重写优先级高于刷盘**；

**主进程的命令A**会保存到刷盘缓存区，返回主进程命令已执行。主进程无阻塞感觉（当然主进程fork会有些许阻塞，rdb也一样。对客户端感觉友好）；此时**主进程的命令A**还是在刷盘缓存里，直到rewrite完全结束（文件已压缩成功且写入到磁盘）后，进行刷盘操作。

若rewirte写入磁盘过程中宕机，**主进程的命令A会丢失**，但却告诉客户端已执行成功。造成数据不一致

**优缺点：** 不安全，命令可能会遗漏（宕机情况下），不能保证数据一致性**；**但是客户端感觉较好。



**优化方案：** 

保证数据一致性：no-appendfsync-on-rewrite no 
保证用户感受：关闭自动重写，用户低峰期 。如用户使用较少的凌晨，每天凌晨定时执行bgrewriteaof。



#### 触发AOF操作

* appendfsync  always/everysec
* flushall
* shutdown
* BGREWRITEAOF(重写)



#### 恢复

1.在home bin 下检测aof文件

```
D:\SOFT_WORK\redis\Redis3>redis-check-aof --fix appendonly.aof
AOF analyzed: size=52, ok_up_to=52, diff=0
AOF is valid
```

2.启动redis









## 六、redis 集群







## 七、redis 应用案例

### 简单应用

####  序列化

#### springboot templete

#### redisUtil(苞米豆)



### 从数据机构

####  string 应用案例

* token 
* 统计次数
* 多机抢占锁
* 临界资源加锁。（当前用户id和资源id组成，比代码锁快，且容易释放）

####  list 应用案例 

//todo



### 从场景

* 多级抢占锁
* 临界资源加锁





### 锁

#### 多机应用 单机redis



#### 多机应用 集群redis