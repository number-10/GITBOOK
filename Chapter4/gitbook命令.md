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
    


   链接：https://www.jianshu.com/p/02caaaaa97ef


