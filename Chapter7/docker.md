# docker-windows



#### Docker桌面

Docker Desktop是一个本机应用程序，可将所有Docker工具提供给您的Mac或Windows计算机。 

1. 打开Docker桌面。（如果没有，请[在此处](https://www.docker.com/products/docker-desktop)下载）。
2. 在终端中键入以下命令： `docker run -dp 80:80 docker/getting-started`
3. 打开浏览器到[http：// localhost](http://localhost/)
4. 玩得开心！





https://docs.docker.com/engine/install/centos/



## 深思

# [Docker为何需要OS的基础镜像？](https://www.cnblogs.com/weiyiming007/p/12875631.html)



原文：https://blog.csdn.net/krismile__qh/article/details/99883273

所有docker容器都是使用的宿主机的OS。这一点和Virtual Machine会虚拟化出Guest OS完全不同。你可以试一下，假设你的

宿主机是Ubuntu 16.04的，内核版本是4.9。不管你运行哪个版本的ubuntu/cent OS容器，看内核版本都是4.9。



共用Host OS是docker的特点！

相比Virtual Machine的虚拟化，Docker更轻，更小（毕竟不用再启动一套内核了嘛）

但当然也有限制，比如Virtual Machine可以 Windown on Linux或者相反，而Docker是不行的。



**问题一**，Container内需不需要OS？
Container不是一个VM技术，所以和OS没有关系。Container指的是Docker Run出的运行环境，因为在里面我们可以运行一些命令，

让使用者以为它就是一个完整的OS环境，这是不对的。其实Docker只是一个进程。当你使用docker exec登录进去的也只是一个

Terminal的模拟环境。它不是真实的OS。正因为它不是OS，所以它是直接调用主机的Kernel的。而Container本身只是一个系统进程。



**第二个问题**：为何需要OS的基础镜像？
首先，OS的问题上面已经解释过了，它不是一个OS，但为何需要OS的基础镜像？其实这里的基础镜像是一个包含rootfs的镜像。

Kernel启动后是需要把启动文件解压到rootfs上的，然后kernel找到init文件启动就可以得到一个Linux环境了，Docker做的事情就

是模拟这个过程，让kernel给出一个独立的隔离环境。 