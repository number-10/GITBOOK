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

  		%MONGODB_HOME$\bin\mongod.exe   --config  .\mongod.cfg --port [port]  --auth --logappend

         #### 		**登录**

​          mongo --port [port]

  		#### 		创建用户 注意空格todo

​			db.createUser({

​			"user","root",

​			"pwd","root",

​			"roles":[{"role":"root","db","xxdb"}]

​			};



​          **验证用户**

​		先切换到所属数据库再验证  

​         use admin;

​          db.auth([user],[pwd]);



//展示库

show dbs;

#### auth

### 3-2 操作级基本应用

//展示集合

show collections;、



//创建集合，通过向集合里插入一条数据

db.myCollection1.insertOne({x:1}); 

myCollection1为新建的集合名称

//查询集合

db.getCollection("myCollection1").find();







## 4、高级运用



## 5、原理分析



