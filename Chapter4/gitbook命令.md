# 第3节：gitbook
## gitbook 环境安装
 1、GitBook 是基于 Node.js，安装node.js略链接。现在安装 Node.js 都会默认安装 npm（node 包管理工具）
 2、npm install -g gitbook-cli
 ```
 C:\Users\number10>npm install -g gitbook-cli
 C:\Users\number10\AppData\Roaming\npm\gitbook -> C:\Users\number10\AppData\Roaming\npm\node_modules\gitbook-cli\bin\gitbook.js
 + gitbook-cli@2.3.2
 added 578 packages from 672 contributors in 26.369s
 ```
 3、查看gitbook版本 **gitbook -V ** -V是大写

## 基本命令
  * 初始化gitbook,
    ```gitbook init```
    执行完后，你会看到多了两个文件 —— README.md 和 SUMMARY.md，它们的作用如下：
  ```
   README.md —— 书籍的介绍写在这个文件里
   SUMMARY.md —— 书籍的目录结构在这里配置
  ```
  目录写后再次 gitbook init gitbook会查找 summary.md 里的目录和文档，建立相应文档。有的话跳过。

  * 构建和运行
  ```
    gitbook build
    gitbook serve --port [port]
  ```
   --port [port] 可省略。默认端口号4000  。静态网站输出到 _book 
    

## 同时运行多个服务

* 需要修改监听端口和服务端口  默认监听端口35729，服务端口4000

 ```  
 gitbook serve --lrport 60000 --port 4001 
 ```

否则可能的错误。faq:

1

Error: listen EADDRINUSE: address already in use :::35729

You already have a server listening on 35729
You should stop it and try again.

2

RangeError [ERR_SOCKET_BAD_PORT]: Port should be >= 0 and < 65536. Received 352730.

## 发布相关book.json
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
### 目录折叠
*1 在主目录里创建 book.json 加插件 chapter-fold 或者 expandable-chapters
*2 在主目录下使用 gitbook install 
### 去掉 本书使用gitbook发布 
*1 在主目录下新加 style目录，再新加website.css文件。内容为：
```
 .gitbook-link {
      display: none !important;
  }
```
*2 在主目录下使用 gitbook install 

   链接：https://www.jianshu.com/p/02caaaaa97ef


