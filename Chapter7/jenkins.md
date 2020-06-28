# jenkins

## 一、安装 

### windows 版

下载列表：http://mirrors.jenkins.io/war-stable/
下载地址:http://mirrors.jenkins.io/war-stable/latest/jenkins.war


* 1 Jenkins的Web应用程序Archive（WAR）文件版本可以安装在任何支持Java的操作系统或平台上。


* 2 将最新的稳定Jenkins WAR文件下载 到计算机上的适当目录。

* 3 打开终端/命令提示符窗口，进入下载目录。

* 4 运行命令java -jar jenkins.war。

* 5浏览http://localhost:8080并等待，直到出现“ 解锁Jenkins”页面。（admin/admin）

继续下面的安装后设置向导。



笔记：

与在Docker中使用Blue Ocean下载和运行Jenkins不同（上述），此过程不会自动安装Blue Ocean功能，而需要通过Jenkins中的Manage Jenkins > Manage Plugins页面单独安装 。在Blue Ocean 入门 页面上阅读有关安装Blue Ocean的详细信息 。

您可以--httpPort在运行java -jar jenkins.war命令时通过指定选项 来更改端口。例如，要使Jenkins通过端口9090可访问，请使用以下命令运行Jenkins：
java -jar jenkins.war --httpPort=9090

### 安装linux版-基于docker

[官网](https://www.jenkins.io/doc/book/installing/)

**1、安装&运行**

```
docker container run --name jenkins-blueocean --rm --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --publish 9200:8080 --publish 50000:50000 jenkinsci/blueocean
 
```

会自动下载。注：目录在docker里的“操作系统”里。命令exec -it 可进入

```
docker exec -it 容器id /bin/bash
```

ip:9200 即可访问，密码先进入docker.在/var/jenkins_home/secrets/initialAdminPassword

**2、配置jdk,git,maven等**

进去jenkins界面。Manage Jenkins->Global Tool Configuration

方案一：在jenkins容器里安装/找到所需要的软件，

```
[root@xxx ~]# docker exec -it  774 /bin/bash
bash-4.4$ echo $JAVA_HOME
/usr/lib/jvm/java-1.8-openjdk
bash-4.4$ which git
/usr/bin/git
bash-4.4$ 
```

node.js  安装插件。进入容器且有权限``` docker exec   --privileged=true -it d85 /bin/bash``` 全局配置命令

ln -s  /var/jenkins_home/tools/jenkins.plugins.nodejs.tools.NodeJSInstallation/nodejs/bin/*  /usr/bin

npm install -g gitbook-cli







方案二: 挂载容器外（linux服务器上的软件）



### 安装linux版

红帽/ CentOS   [官网链接](https://www.jenkins.io/doc/book/installing/#red-hat-centos)

您可以通过`yum`在Red Hat Enterprise Linux，CentOS和其他基于Red Hat的发行版上安装Jenkins 。您需要选择Jenkins长期支持版本或Jenkins每周版本。

##### 长期支持版本

每12周从定期发布流中选择一个[LTS（长期支持）](https://www.jenkins.io/download/lts/)发布作为该时间段的稳定发布。可以从[`redhat-stable`](https://pkg.jenkins.io/redhat-stable/)yum存储库安装。

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade
sudo yum install jenkins java-1.8.0-openjdk-devel
```

默认安装路径/usr/lib/jenkins/

启动 systemctl start jenkins

若失败 可到系统配置处执行  ```/etc/init.d/jenkins start```

**改配置**

* 用户： 如下
* 端口：/etc/sysconfig/jenkis 文件里的 JENKINS_PORT="8080" 

FAQ：



**Q:启动无反应/失败**

A:改/etc/sysconfig/jenkins（此文件是从/etc/init.d/jenkins里获取的）里面的用户

 JENKINS_USER="jenkins" 改为

 JENKINS_USER="root"



**Q2:显示Starting jenkins (via systemctl):                          [  OK  ]  但仍未启动。**

 A2:若ps -ef|grep xxx 无此进程，仍启动不了。

一般要删除对应的pid文件和lock文件。

具体目录在配置文件中。**PID_FILE ： /var/run/*.pid*    ； LOCKFILE ：/var/lock/subsys/*  **

JENKINS_PID_FILE="/var/run/jenkins.pid"
JENKINS_LOCKFILE="/var/lock/subsys/jenkins"

可用stop命令 删除 LOCKFILE.再启动

## 二、构建项目

**一些插件**：

* .**maven插件**：管理jenkins->管理插件->可选插件->安装maven插件。名称是[Maven Integration](https://plugins.jenkins.io/maven-plugin)  翻译：maven整合。文档编写时是3.6。点击下方的直接安装

* **git 插件**

* **node.js 插件 ：**[NodeJS](https://plugins.jenkins.io/nodejs)

  NodeJS Plugin executes [NodeJS](http://nodejs.org/) script as a build step.

[官方文档参考](https://github.com/jenkinsci/pipeline-plugin/blob/master/TUTORIAL.md#understanding-flow-scripts)



**Faq:**

Q:java.io.IOException: CreateProcess error=2, 系统找不到指定的文件。

A:pipeline script 若在windwos上 命令应用bat而不是sh

