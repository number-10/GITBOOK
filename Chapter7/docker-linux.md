# docker

## 一、下载安装

https://docs.docker.com/engine/install/centos/

查看版本、信息 docker version     docker info

默认安装路径： /var/lib/docker

配置文件：/usr/lib/systemd/system  docker.service 



更改修改 /var/lib/docker 的存储路径：

配置文件 ExecStart=/usr/bin/dockerd **--graph [newFolder]**

## 二、添加用户

因为按 按照后已有docker组。所有要新加用户即可，要指定组

```    
useradd docker -g docker
passwd [密码]
```

-g GID/GROUPNAME：指定默认组，可以是 GID 或者 GROUPNAME，同样也必须真实存在

前一个docker为用户名，后一个docker为组名。



## 三、基本命令





root用户。直接

* 查看运行状态

```
systemctl status docker
```

* 启动

```
systemctl start docker
```

* 关闭

```
systemctl stop docker
```

* 测试

 ```
docker run hello-world
 ```



____

***

* **查看docker 里安装的软件目录配置，**如 jenkins ，redis 等。

  ```
  docker exec -it 容器id /bin/bash
  ```

  这些容器依赖于新的"操作系统"，docker 依赖centons7内核生成的。每个软件都对应一个“操作系统”。本质是宿主机进程

  **进入容器且有权限**：如创建文件，软连接等

  ```ker exec   --privileged=true -it d85 /bin/bash``` 全局配置命令

  

* 查看所有本地镜像

  ```    
  # docker images
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  hello-world         latest              bf756fb1ae65        5 months ago        13.3kB
  ```

  

* 删除本地镜像
  `docker rmi image-id`

* 查找镜像

  `docker search tomcat` //查找tomcat镜像

  #### 容器操作

  3、查看运行中的容器：docker ps
  4、查看所有的容器：docker ps -a
  5、**停止/启动运行中的容器： docker stop/start 容器id/容器名**
  6**、删除容器：docker rm 容器id/容器名**。 注意：删除前要先停止容器
  7、端口映射：
  -p 8080:8080
  //-p:表示将主机的端口映射到容器中的端口
  eg: docker run -d -p 8888:8080 tomcat //--name 可以省略
  8、查看容器的日志： docker logs 容器id/name
  
  9 、 改容器名 :  docker rename CONTAINER NEW_NAME

其它用户 

在上述命令加上sudo。

若是用docker用户，sudo省略。前提是要验证

```
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to manage system services or units.
Authenticating as: root
Password: 
==== AUTHENTICATION COMPLETE ===
```



docker 容器内安装yum

```
apt-get update
apt-get install vim -y
apt-get install yum -y
```







### 四、以非root用户身份运行Docker守护程序（无根模式）

无根模式允许以非root用户身份运行Docker守护程序和容器，以减轻守护程序和容器运行时中的潜在漏洞。

只要满足[先决条件](https://docs.docker.com/engine/security/rootless/#prerequisites)，即使安装Docker守护程序，无根模式也不需要root特权。

Docker Engine 19.03中引入了无根模式。

> **注意**：无根模式是一项实验性功能，具有[局限性](https://docs.docker.com/engine/security/rootless/#known-limitations)。

####  如何运作

无根模式在用户名称空间内执行Docker守护程序和容器。这与[`userns-remap`mode](https://docs.docker.com/engine/security/userns-remap/)非常相似，不同之处在于，在`userns-remap`mode模式下，守护进程本身以root特权运行，而在无根模式下，守护进程和容器都在没有root特权的情况下运行。

无根模式不使用具有SETUID位或文件功能的二进制文件，但`newuidmap`和除外`newgidmap`，它们是必需的，以便允许在用户名称空间中使用多个UID / GID。

#### 前提条件

- `newuidmap`并`newgidmap`需要安装在主机上。这些命令由`uidmap`大多数发行版的软件包提供。
- `/etc/subuid`且`/etc/subgid`应至少包含该用户的65,536个从属UID / GID。在以下示例中，用户`testuser`具有65,536个从属UID / GID（231072-296607）。

```
$ id -u
1001
$ whoami
testuser
$ grep ^$(whoami): /etc/subuid
testuser:231072:65536
$ grep ^$(whoami): /etc/subgid
```



## 五、例子 nginx在容器中运行

* **1 **   查看获取nginx 镜像
* https://hub.docker.com/_/nginx?tab=tags

```
docker search nginx
docker pull nginx:latest
docker images
```

* **2** 运行 nginx 容器：

```
$ docker run --name nginx-test -p 9901:80 -d  d-nginx  
$ docker images
```

参数说明：

- **--name nginx-test**：容器名称（ 自设）。
- **- p 9901:80**： 端口进行映射，将本地 9901端口映射到容器内部的 80 端口。
- **-d nginx**： 设置容器在在后台一直运行。

  



* **3** nginx容器 参数配置：

