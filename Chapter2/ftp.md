#             linux ftp账号

### 1、ftp 账号创建

 创建用户，并指定主目录，即能访问的目录

```
 useradd -d /home/ftpuser2 -m ftpuser2
 passwd ftpuser2 
 
```

例：

```basic
[root@iZ2ze56pkpg6dxap2z3qvzZ /]# cd /home
[root@iZ2ze56pkpg6dxap2z3qvzZ home]# ls
a  ftpuser1  web_card
[root@iZ2ze56pkpg6dxap2z3qvzZ home]# useradd -d /home/ftpuser2 -m ftpuser2
[root@iZ2ze56pkpg6dxap2z3qvzZ home]# ls
a  ftpuser1  ftpuser2  web_card
[root@iZ2ze56pkpg6dxap2z3qvzZ home]# cd ftpuser2
[root@iZ2ze56pkpg6dxap2z3qvzZ ftpuser2]# ls
[root@iZ2ze56pkpg6dxap2z3qvzZ ftpuser2]# passwd ftpuser2 
Changing password for user ftpuser2.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@iZ2ze56pkpg6dxap2z3qvzZ ftpuser2]# 
```



### 2、ftp权限（ftp主目录）

##### 2-1修改主目录

法一：

注：此方法只能新增一个主目录。要把原先的删掉，不建议使用

```
usermod -d /home/ftpuser2new ftpuser2
```



法二：

注：此方法能更改用户目录且把原来的内容移动到新目录。原来的目录会消失。建议使用此方法

```
usermod -m -d /home/ftpuser2new  -u 1003 ftpuser2

usermod -d /home/ftpuser2new   -m  -u 1003 ftpuser2
```

1003 为用户id。cat /etc/passwd 命令可查看



可能遇到的问题

```
usermod: user ftpuser2 is currently used by process 15621
```

关闭ftp工具的软件,或者ps -ef|grep ftpuser2  查看进程。再kill -9 pid 。



##### 2-2 禁用ssh登录

```
usermod -s /sbin/nologin ftpuser2
```

恢复ssh登陆可用以下命令：

usermod -s /sbin/bash ftpuser2

可能遇到的问题：

恢复后登录不上ftp问题 /todo

先用 cat /etc/passwd 看下其它用户是什么，若是bin/bash .则用下面命令即可

```
usermod -s /bin/bash ftpuser2
```



ftpuser1​：x:1002:1002::/home/ftpuser1:/bin/bash

##### 2-3 只能上传，下载，不能删除

/todo 123123

##### 2-4 除本身主目录外还能访问其他目录（绑定挂载方案）

/todo 123123

other是新目录 重要，挂载后 挂载点会隐藏挂载前的子文件级目录

mkdir otherdir

mount --bind /home/ftp1 otherdir

这样FTP登录账号就可以访问otherdir了。



mount --bind  /home/ftp_gjj/person  /data/SZGJJ/PUT_HERE






### 3、ftp 主动被动模式







