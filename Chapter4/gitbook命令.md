# 第3节：gitbook
## 一、gitbook 环境安装
 1、GitBook 是基于 Node.js，安装node.js略链接。现在安装 Node.js 都会默认安装 npm（node 包管理工具）
 2、npm install -g gitbook-cli
 ```
 C:\Users\number10>npm install -g gitbook-cli
 C:\Users\number10\AppData\Roaming\npm\gitbook -> C:\Users\number10\AppData\Roaming\npm\node_modules\gitbook-cli\bin\gitbook.js
 + gitbook-cli@2.3.2
 added 578 packages from 672 contributors in 26.369s
 ```
 3、查看gitbook版本 **gitbook -V ** -V是大写 or **gitbook current**  、 **gitbook ls**

 4、 查看网上可用的服务器版本  gitbook  ls-remote

```
$ gitbook ls-remote
Available GitBook Versions:

     4.0.0-alpha.6, 4.0.0-alpha.5, 4.0.0-alpha.4, 4.0.0-alpha.3, 4.0.0-alpha.2, 4.0.0-alpha.1, 3.2.3, 3.2.2, 3.2.1, 3.2.0, 3.2.0-pre.1, 3.2.0-pre.0, 3.1.1, 3.1.0, 3.0.3, 3.0.2, 3.0.1, 3.0.0, 3.0.0-pre.15, 3.0.0-pre.14, 3.0.0-pre.13,
 3.0.0-pre.12, 3.0.0-pre.11, 3.0.0-pre.10, 3.0.0-pre.9, 3.0.0-pre.8, 3.0.0-pre.7, 3.0.0-pre.6, 3.0.0-pre.5, 3.0.0-pre.4, 3.0.0-pre.3, 3.0.0-pre.2, 3.0.0-pre.1, 2.6.9, 2.6.8, 2.6.7, 2.6.6, 2.6.5, 2.6.4, 2.6.3, 2.6.2, 2.6.1, 2.6.0, 2.
5.2, 2.5.1, 2.5.0, 2.5.0-beta.7, 2.5.0-beta.6, 2.5.0-beta.5, 2.5.0-beta.4, 2.5.0-beta.3, 2.5.0-beta.2, 2.5.0-beta.1, 2.4.3, 2.4.2, 2.4.1, 2.4.0, 2.3.3, 2.3.2, 2.3.1, 2.3.0, 2.2.0, 2.1.0, 2.0.4, 2.0.3, 2.0.2, 2.0.1, 2.0.0, 2.0.0-beta
.5, 2.0.0-beta.4, 2.0.0-beta.3, 2.0.0-beta.2, 2.0.0-beta.1, 2.0.0-alpha.9, 2.0.0-alpha.8, 2.0.0-alpha.7, 2.0.0-alpha.6, 2.0.0-alpha.5, 2.0.0-alpha.4, 2.0.0-alpha.3, 2.0.0-alpha.2, 2.0.0-alpha.1

Tags:

     latest : 2.6.9
     pre : 4.0.0-alpha.6

```

`gitbook fetch` 下载 和 `gitbook update`升级,两种方式都可以体验最新版本,这里选择下载方式方便进行不同版本的切换.

```
gitbook fetch 4.0.0-alpha.6
```



## 二、基本命令
  * **初始化gitbook** ,
    ```gitbook init```
    执行完后，你会看到多了两个文件 —— README.md 和 SUMMARY.md，它们的作用如下：
  ```
   README.md —— 书籍的介绍写在这个文件里
   SUMMARY.md —— 书籍的目录结构在这里配置
  ```
  目录写后再次 gitbook init gitbook会查找 summary.md 里的目录和文档，建立相应文档。有的话跳过。

  * **构建和运行**
  ```
    gitbook build
    gitbook serve --port [port]
  ```
   --port [port] 可省略。默认端口号4000  。静态网站输出到 _book 
    

## 三、进阶命令

### 1、同时运行多个服务，指定监听端口，服务端口

* 需要修改监听端口和服务端口  默认监听端口35729，服务端口4000

 ```  
 gitbook serve --lrport 60000 --port 4001 
 ```

否则可能的错误。faq:

**a、**Error: listen EADDRINUSE: address already in use :::35729

You already have a server listening on 35729
You should stop it and try again.

**b、**RangeError [ERR_SOCKET_BAD_PORT]: Port should be >= 0 and < 65536. Received 352730.



### 2、调试，指定版本运行

```
gitbook serve --gitbook=3.23 --log=debug
```



## 四、发布展示相关

 **配置 book.json**

```
{
  "title": "mybook",
  "author" : "xf",
  "description": "desc",
  "language": "zh-hans",
  "styles": {
	"website": "styles/website.css"
  },
  "plugins": [ 
	"chapter-fold"
  ]
}
```



### 1、目录折叠
* 1 在主目录里创建 book.json 加插件 **chapter-fold** 或者 **expandable-chapters**
* 2 在主目录下使用 gitbook install 

### 2、去掉 “本书使用gitbook发布”
* 1 在主目录下新加 styles目录，再新加website.css文件。website.css内容为：

```
 .gitbook-link {
      display: none !important;
  }
```
* 2  book.json 加配置

  ```
  "plugins": [ 
  	"chapter-fold"
    ]
  ```

   

### 3、PDF发布

   //todo

### 4、电子书发布

//todo

### 5、发布到服务器





链接：https://www.jianshu.com/p/02caaaaa97ef


