# NGINX

[谈谈中间件开发 - 莫那-鲁道 - 博客园](https://www.cnblogs.com/stateis0/p/9822281.html)



[TOC]



## 1、文档

[NGINX Docs | Serving Static Content](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/)

## 二、安装 启动

https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#

[Nginx Linux详细安装部署教程 - taiyonghai - 博客园](http://www.cnblogs.com/taiyonghai/p/6728707.html)

### 帮助命令

```
nginx -h
```

-s signal     : send signal to a master process: stop, quit, reopen, reload  或向主进程发送信号:

`nginx -s quit` –正常关毕

`nginx -s stop` –立即关闭（快速关闭）

nginx -s reload –重新加载配置文件

`nginx -s reopen` –重新打开日志文件

### 检查安装是否成功

    [root@iz2ze56pkpg6dxap2z3qvzz ~]# nginx -v
    nginx version: nginx/1.17.0

#### 设置检测配置文件

```
nginx -c /etc/nginx/nginx.conf
```

**检测配置文件 nginx -t**  。

***注：检测的一直是默认的 /usr/local/nginx/conf/nginx.conf 文件。哪怕你设置应用了新的配置文件***

若要检测当前配置的配置文件。**```nginx -t -c /etc/nginx/nginx.conf```**

```
[root@iz2ze56pkpg6dxap2z3qvzz conf.d]# nginx -t -c /etc/nginx/nginx.conf
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

没设置配置文件可能报错如下：

[root@iz2ze56pkpg6dxap2z3qvzz nginx]# nginx -s reload
nginx: [error] open() "/usr/local/nginx/logs/nginx.pid" failed (2: No such file or directory)

设置即可解决





#### [配置文件所在目录官网建议](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/):

/etc/nginx/nginx.conf 主功能配置文件

/etc/nginx/conf.d   特定功能的配置文件
在nginx.conf  里写include 功能特定的配置文件，例子: 

```
events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    
    #导入conf.d下的配置文件
    include ./conf.d/*.conf;
    
    ****省略****
```

#### 查看当前主机 nginx 相关配置

##### 查看配置文件所在目录

* **ps -ef | grep nginx   master process里的conf**

  ```
[root@iz2ze56pkpg6dxap2z3qvzz ~]# ps -ef|grep nginx
    root      1871     1  0 Feb03 ?        00:00:00 nginx: master process nginx -c /etc/nginx/nginx.conf
    nobody    7262  1871  0 Feb04 ?        00:00:00 nginx: worker process
    nobody    7263  1871  0 Feb04 ?        00:00:00 nginx: worker process
    
  ```

* **locate nginx.conf**

##### 查看启动文件所在目录

* 找到nginx PID 号 。netstat -lntup|grep nginx  或  ps -ef|grep nginx 

  ll  /proc/PID/exe

  ```
  [root@iz2ze56pkpg6dxap2z3qvzz ~]# ll /proc/1871/exe
  lrwxrwxrwx 1 root root 0 Feb  3 16:32 /proc/1871/exe -> /usr/local/nginx/sbin/nginx
  ```

  

### 启动命令

```
nginx -s  reload
```

relaod 1重新加载配置文件 2平滑启动  先关闭（会等待已有的链接结束，新的不准进），再启动

nginx -s  reopen  重新打开日志文件  //【【【【【【 todo】】】】】 

### 关闭命令

```
nginx -s quit
nginx -s stop
```

 区别： quit 正常关闭（会等待正在交换的连接）。 stop 快速关闭 （不管有没有正在交互的连接）

### 查看nginx运行情况

**```ps -ef | grep nginx```**

 **```netstat -tunlp | grep  nginx```**

```
[root@iz2ze56pkpg6dxap2z3qvzz conf.d]# ps -ef|grep nginx
root      1871     1  0 16:32 ?        00:00:00 nginx: master process nginx -c /etc/nginx/nginx.conf
nobody    1902  1871  0 16:38 ?        00:00:00 nginx: worker process
root      1936  1809  0 16:41 pts/3    00:00:00 grep --color=auto nginx

[root@iz2ze56pkpg6dxap2z3qvzz conf.d]# netstat -tunlp | grep nginx
tcp        0      0 0.0.0.0:8888            0.0.0.0:*               LISTEN     1871/nginx: master  
tcp        0      0 0.0.0.0:8000            0.0.0.0:*               LISTEN      1871/nginx: master  
tcp        0      0 0.0.0.0:8001            0.0.0.0:*               LISTEN      1871/nginx: master  
tcp        0      0 0.0.0.0:8100            0.0.0.0:*               LISTEN      1871/nginx: master  
tcp        0      0 0.0.0.0:18888           0.0.0.0:*               LISTEN      1871/nginx: master  
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      1871/nginx: master  
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1871/nginx: master  
[root@iz2ze56pkpg6dxap2z3qvzz conf.d]# 
```



### nginx master 进程和worker 进程**

//tood【【【【【】】】】】




## 三、nginx conf 参数



### 基础参数

* 一些顶级指令（称为***context**）*将适用于不同流量类型的指令分组在一起：

* [`events`](https://nginx.org/en/docs/ngx_core_module.html#events) –常规连接处理
* [`http`](https://nginx.org/en/docs/http/ngx_http_core_module.html#http) – HTTP流量
* [`mail`](https://nginx.org/en/docs/mail/ngx_mail_core_module.html#mail) –邮件流量
* [`stream`](https://nginx.org/en/docs/stream/ngx_stream_core_module.html#stream) – TCP和UDP流量

放置在这些上下文之外的指令被称为在*主要*上下文中。

**[指令释义链接 来源官方](https://nginx.org/en/docs/http/ngx_http_proxy_module.html?&_ga=2.222125080.1492117495.1612338613-1574912474.1574297231#directives)**

 #### backup

  upstream bakend{#定义负载均衡设备的Ip及设备状态}{
  ip_hash;
  server 127.0.0.1:9090 down;
  server 127.0.0.1:8080 weight=2;
  server 127.0.0.1:6060;
  server 127.0.0.1:7070 backup;
  }
  在需要使用负载均衡的server中增加
  proxy_pass http://bakend/;

  每个设备的状态设置为:
  1.**down**表示单前的server暂时不参与负载
  2.weight为weight越大，负载的权重就越大。
  3.max_fails：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream模块定义的错误
  4.fail_timeout:max_fails次失败后，暂停的时间。
  5.**backup**： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。



#### location 正则

* **/** 匹配任何查询（因为所有请求都已 / 开头）。

  但是正则表达式规则和长的块规则将被优先和查询匹配。**优先级低**

* **=** 严格匹配。如果这个查询匹配，那么将停止搜索并立即处理此请求。

* **~** 为区分大小写匹配(可用正则表达式)
* **~*** 为不区分大小写匹配(可用正则表达式)

* **!~**为区分大小写不匹配

* **!~***为不区分大小写不匹配

* **^~** 如果把这个前缀用于一个常规字符串,那么告诉nginx 如果路径匹配那么不测试正则表达式。

  **优先级高于/和普通正则表达式**。即 location ^~ /images/ {
  匹配任何已 /images/ 开头的任何查询并且停止搜索。任何正则表达式将不会被测试

  

## 四、代理相关

### http/https请求相关

#### 请求头参数

**[proxy_set_header](https://nginx.org/en/docs/http/ngx_http_proxy_module.html?&_ga=2.222125080.1492117495.1612338613-1574912474.1574297231#proxy_set_header)**



#### 跨域设置

```
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header Access-Control-Allow-Origin *;
```

* [Nginx 跨域设置 Access-Control-Allow-Origin 无效的解决办法_frank_passion的专栏-CSDN博客](https://blog.csdn.net/frank_passion/article/details/53898769?>)



#### https ssl证书配置

```
server {
        server_name localhost;

        listen 80   proxy_protocol;
        listen 443  ssl proxy_protocol;

        ssl_certificate      /etc/nginx/ssl/public.example.com.pem;
        ssl_certificate_key  /etc/nginx/ssl/public.example.com.key;

        location /app/ {
            proxy_pass       http://backend1;
            proxy_set_header Host            $host;
            proxy_set_header X-Real-IP       $proxy_protocol_addr;
            proxy_set_header X-Forwarded-For $proxy_protocol_addr;
        }
    }
```

ssl_certificate      /etc/nginx/ssl/public.example.com.crt;
 ssl_certificate_key  /etc/nginx/ssl/public.example.com.key;

### **负载均衡方法 Load-Balancing Method **

[官网文档链接](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)

**注意：upstream 里的服务地址不能加http，proxy_pass 里一定要加http:// or https://**

```
http {
    upstream backend {
        server backend1.example.com;
        server backend2.example.com;
        server 192.0.0.1 backup;
    }
    
    server {
        location / {
            proxy_pass http://backend;
        }
    }
}
```



#### 1、Round Robin（默认）  轮询

请求在服务器之间平均分配，同时考虑了[服务器权重](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#weights)。默认情况下使用此方法（没有启用它的指令）：

```
upstream backend {
   # no load balancing method is specified for Round Robin
   server backend1.example.com;
   server backend2.example.com;
}
```

默认权重是1

#### 1-2、Server Weights 服务器权重

```
upstream backend {
    server backend1.example.com weight=5;
    server backend2.example.com;
    server 192.0.0.1 backup;
}
```

默认权重是1

按比例分发请求

#### 2、Least Connections 最少连接

将请求发送到具有最少活动连接数的[服务器](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#weights)，同时还要考虑[服务器权重](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#weights)：

```
upstream backend {
    least_conn;
    server backend1.example.com;
    server backend2.example.com;
}
```

默认权重是1

#### 3、IP Hash  

从客户端IP地址确定向其发送请求的服务器。在这种情况下，可以使用IPv4地址的前三个八位字节或整个IPv6地址来计算哈希值。该方法保证了来自相同地址的请求将到达同一服务器，除非该请求不可用。

```
upstream backend {
    ip_hash;
    server backend1.example.com;
    server backend2.example.com;
}
```

**适用场景：**

* 无session集群配置的多机部署方案，对于同一个用户的session信息需要保存在同一台机器上，否则就要重新登录了。

* 分片上传大文件，需要上传到同一服务器进行文件合并，再进行其它操作

  

#### 4、Generic Hash 通用哈希

将请求发送到的服务器是根据用户定义的键确定的，该键可以是文本字符串，变量或组合。例如，密钥可以是成对的源IP地址和端口，或者是本示例中的URI

```
upstream backend {
    hash $request_uri consistent;
    server backend1.example.com;
    server backend2.example.com;
}
```

$request_uri  可换成其它变量。consistent 可去掉

伪指令的可选[`consistent`](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#hash)参数`hash`启用[ketama](https://www.last.fm/user/RJ/journal/2007/04/10/rz_libketama_-_a_consistent_hashing_algo_for_memcache_clients)一致性哈希负载平衡。根据用户定义的哈希键值，请求将平均分布在所有上游服务器上。如果将上游服务器添加到上游组或从上游组中删除，则只有少数几个键会被重新映射，从而在负载平衡缓存服务器或其他累积状态的应用程序的情况下最大程度地减少缓存丢失。

**ketama** //todo【【【【【】】】】】 https://blog.csdn.net/sdausxc/article/details/52059608 //todo[[[[]]]]



**url hash(hash $request_uri) 适用场景：**

* 用户下载文件方案（云存储->各个服务器主机->用户），因为用户有可能多次下载同一个文件。为了提高速度把文件缓存到服务器主机上。

  但是下一次下载文件请求有可能到其它主机上了，为了提高缓存命中率，节约空间时间。要保证同一用户下载文件请求在同一个主机上。



#### 5、[**Least Time**](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#least_time) (NGINX Plus only)  最短响应时间，仅NGINX Plus可配置。

  nginx 配置，检测配置文件会报 unknown directive 未失败的指定

```nginx: [emerg] unknown directive "least_time" in /etc/nginx/./conf.d/gitbook.conf:62```

```nginx: configuration file /etc/nginx/nginx.conf test failed```



```
upstream backend {
    least_time header;
    server backend1.example.com;
    server backend2.example.com;
}
```

- `header` –从服务器接收第一个字节的时间
- `last_byte` –是时候从服务器接收完整响应了
- `last_byte inflight` –考虑到请求不完整的时间，可以从服务器接收完整的响应



#### 5-2、 fair 最短相应时间

按后端服务器的响应时间来分配请求，响应时间短的优先分配。 

```
upstream backserver { 
fair; 
server server1; 
server server2; 
} 
```





#### 6、RANDOM 随机

```
upstream backend {
    random two least_time=last_byte;
    server backend1.example.com;
    server backend2.example.com;
    server backend3.example.com;
    server backend4.example.com;
}
```

- `least_conn` –活动连接最少
- `least_time=header`（NGINX Plus）–从服务器接收响应标头的最短平均时间（[`$upstream_header_time`](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#var_upstream_header_time)）
- `least_time=last_byte`（NGINX Plus）–从服务器接收完整响应的最短平均时间（[`$upstream_response_time`](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#var_upstream_response_time)）

**适用场景：**

的**随机**负载平衡方法应被用于在多个负载平衡器传递请求到相同组的后端分布式环境。对于负载平衡器具有所有请求的完整视图的环境，请使用其他负载平衡方法，例如轮询，最少连接和最少时间。



## 五、集群，高可用

 用keepalived和nginx方案。//todo

**大概流程：**  用户到nginx1,nginx2,...。 变成用户到中转站/虚拟地址/组播地址 到 主nginx（其中一个）。



中转站如何选择其中一个nginx：

keepalived类似于路由。虚拟路由冗余协议(Virtual Router Redundancy Protocol，简称VRRP)。
方案：每个含义nginx主机都有keepalived 服务。主机会行成组。一主多从，主挂了，从变主。



## 六、 连接数，常见问题

**用户通过nginx 请求静态资源 消耗2个连接。**（1用户请求nginx、 2nginx把数据给用户）

```
events {
    worker_connections 1024;
}
```

**用户通过nginx 请求动态资源（通过服务取数据。如tomcat,数据库等） 消耗4个连接**。（1用户请求nginx 、2nginx请求服务、3服务把数据给nginx 4nginx把数据给用户）

总连接数为  工作进程数（worker_processes 1）* 每个worker的连接数（worker_connections）







[nginx 配置add_header Access-Control-Allow-Origin * 依然存在跨域问题_xiojing的博客-CSDN博客](https://blog.csdn.net/xiojing825/article/details/83383524)

[nginx系列之一：nginx入门 - 王道长的技术博客 - CSDN博客](https://blog.csdn.net/qq_29677867/article/details/90112120)

[Nginx简单配置转发 - 还没见过大海 - CSDN博客](https://blog.csdn.net/qq_21856521/article/details/81945458)

[nginx快速查看配置文件的方法 - Bug怪的博客 - CSDN博客](https://blog.csdn.net/aaaaaab_/article/details/88668808)

[nginx启动、重启、关闭 - helloxiaozhe的博客 - CSDN博客](https://blog.csdn.net/helloxiaozhe/article/details/80596138)

[Nginx设置404错误页面跳转 - masteryee的博客 - CSDN博客](https://blog.csdn.net/masteryee/article/details/83689954)

[nginx重启几种方法 - 习惯沉淀 - 博客园](https://www.cnblogs.com/yadongliang/p/9212795.html)

[nginx 域名跳转一例~~~(rewrite、proxy) - Leone- - 博客园](https://www.cnblogs.com/doseoer/p/5291739.html)

[SpringBoot 实现前后端分离的跨域访问（Nginx） - 简书](https://www.jianshu.com/p/520021853827)

[windows下nginx的安装及使用 - 将王相 - 博客园](https://www.cnblogs.com/jiangwangxiang/p/8481661.html)



[(5条消息)swagger用Nginx反向代理之后，出现no response from server错误的解决办法_samt007的Blog-CSDN博客_no response from server](https://blog.csdn.net/samt007/article/details/79846966?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

[(5条消息)用ngnix做反向代理的项目swagger访问不了_wahaha13168的博客-CSDN博客_通过ng代理后,swagger能打开,但是接口请求无端口](https://blog.csdn.net/wahaha13168/article/details/86290169?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)

[(5条消息)nginx启动、重启、重新加载配置文件和平滑升级_运维_蝈蝈的博客-CSDN博客](https://blog.csdn.net/gnail_oug/article/details/52754491)




