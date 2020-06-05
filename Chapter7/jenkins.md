# jenkins

## 安装 windows 版

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

