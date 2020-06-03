# MongoDB

### 感慨

MongoDB适用于文件，是nosql。**但本质仍是数据库**

### 用自己以前的思维模型理解

库还是库，集合等于表，文档相当于表的一行数据，记录。

## 1、简介

略

## 2、安装

见p1

## 3、基本运用

### 	3-1 系统级基本应用

#### 		**启动服务，-windows**

  	```	
  		%MONGODB_HOME$\bin\mongod.exe   --config  .\mongod.cfg --port [port]  --auth --logappend
  		```

#### 		**登录**

方案一： 客户端连接后再验证

	mongo  --host [host] --port [port]

mongo --host 192.168.100.126 --port 27017 ;

[验证](#jump1)

FAQ:

MongoDB shell version v4.0.10
exception: Bad digit ";" while parsing 27017;

解决方案： ；要和27017 隔开。

方案二：直接验证

```
mongo --host [host] --port [port] -u [user] -p [password]
--authenticationDatabase "admin"
```

mongo --host 192.168.100.126 --port 27017 -u "root" -p "123456" --authenticationDatabase   "admin" ;

mongo --host 192.168.100.126 --port 27017 -u "ghf" -p "linkage@123456"  --authenticationDatabase  "ghfdb" ;



mongo --host 192.168.100.126 --port 27017 -u ghf -p linkage@123456 --authenticationDatabase  ghfdb;

#### 		创建用户 注意空格todo

###### MongoDB基本的角色

1.数据库用户角色：read、readWrite;

- Read：允许用户读取指定数据库   readWrite：允许用户读写指定数据库

2.数据库管理角色：dbAdmin、dbOwner、userAdmin；

- dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile

3.集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；

-  clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。

4.备份恢复角色：backup、restore；
5.所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase

- readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限

- readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限

- userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限

- dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。

6.超级用户角色：root 

只在admin数据库中可用。超级账号，超级权限



```
db.createUser({
			"user":"root",
			"pwd":"root",
			"roles":[{"role":"root","db":"xxdb"}]
});
```

```
db.createUser({
			"user":"ghf",
			"pwd":"linkage@123456",
			"roles":[{"role":"readWrite","db":"ghfdb"}]
});
```

*注：添加完用户后可以使用show users或db.system.users.find()查看已有用户*在当前库

​    use admin; db.system.users.find().pretty()查看所有用户



FAQ: 命令输入完成后，没有执行结果显示，显示的是省略号3个点。
原因：命令没有输入完成，检测命令语句是否完整

<span id="jump1"> 跳转到的地方 </span>


 **验证用户**

先切换到所属数据库再验证  

```
use admin;
db.auth([user],[pwd]);
```

db.auth("ghf","linkage@123456");

db.auth("root","123456");

//展示库

`show dbs;`

#### auth

### 3-2 操作级基本应用

//展示集合

show collections;



//创建集合，通过向集合里插入一条数据

db.myCollection1.insertOne({x:1}); 

myCollection1为新建的集合名称

//查询集合

db.getCollection("myCollection1").find();







## 4、高级运用



## 5、原理分析



