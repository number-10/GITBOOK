#        	node.js 安装

## 一：linux 安装

**方案一：yum**

```
yum install -y nodejs
```

不推荐。此方案 node ，npm 版本过低。有些编译会出错

**方案二： 官网下载二进制文件，配置全局命令**

* **1** [下载地址](https://nodejs.org/en/download/)    选择**Linux Binaries (x64)**

* **2** 文件放到 /usr/local   下，

  解压 、解包 node-v12.18.2-linux-x64.tar.xz  改名称

  ```
  $ xz -d node-v12.18.2-linux-x64.tar.xz
  
  $ tar -xvf node-v12.18.2-linux-x64.tar
  
  $ mv  node-v12.18.2-linux-x64 node12
  ```

  即nodejs home 为   /usr/local/node12

* **3** 配置全局命令

  法一：软链接法

  ```
  ln -s /usr/local/node12/bin/node /usr/bin/node 
  ln -s /usr/local/node12/bin/npm /usr/bin/npm 
  
  [root@iz2ze56pkpg6dxap2z3qvzz bin]# node -v
  v12.18.2
  [root@iz2ze56pkpg6dxap2z3qvzz bin]# npm -v
  6.14.5
  ```

  

  法二：

  vi /etc/profile

  在文件末尾添加

  export NODE_HOME=/usr/local/node12
  export PATH=$PATH:$NODE_HOME/bin 
  export NODE_PATH=$NODE_HOME/lib/node_modules

  

  source /etc/profile

  

**方案三： 官网下载源码安装，编译，配置全局命令**





**FAQ**:

[202007bug单](Chapter31-bug/202007bug单.md)