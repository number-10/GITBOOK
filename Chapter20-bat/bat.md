# 第一节: bat命令


## 一、介绍

cmd命令和bat命令区别：

bat命令：dos解释器解释，

cmd命令 windows cmd.exe 执行。

bat命令在cmd命令环境下可运行，反过来就不行了



# 二、基本命令

帮助命令   --------   [命令] /？

```
rd /?
dir /?
```

echo 打印

set   赋值

%%i 中间变量

* 在脚本里 %%i  在cmd运行时 %i



**删除文件夹命令** 

```
rd /s/q [path]
```

/s 删除目录树，不加的话 只能删除空目录，若目录下有文件，则删除不了

/q 安静模式 ，直接删除，不确认

* 不能使用通配符 *等，rd是rmdir 简写

**删除文件命令**

```
del /s/q *
```



## 三、案例

删除某文件下所有内容（包括文件夹和文件），单不删除文件本身。del.bat 脚本

```
@echo off
:功能 删除spath下的所有内容（文件、文件夹）

:目标文件夹
set spath=F:\a
:删除文件 在cmd里执行命令不行
del /s/q %spath% 
```



```
@echo off
:功能 删除spath下的所有内容（文件、文件夹）

:目标文件夹
set spath=F:\a

:删除文件
del /s/q %spath% 

:遍历删除文件夹
for /f  %%1 in ('dir/s/b/ad %spath%')do (
rd/q/s %%1 
)

```

or 在cms直接执行

```
for /f  %1 in (' dir/s/b/ad f:\a ')do (rd/q/s %1 ) 
```

* **注：%%1 和%1**

