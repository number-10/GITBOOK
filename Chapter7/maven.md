# maven  安装

## 一：linux 安装

**方案一：yum**

```
wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo

yum -y install apache-maven
```



**方案二： 官网下载二进制文件，配置环境变量**

- [下载地址]http://maven.apache.org/download.cgi    

  apache-maven-3.6.3-bin.zip  [apache-maven-3.6.3-bin.zip](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip)

  



**方案三： 官网下载源码安装，编译，配置环境命令**