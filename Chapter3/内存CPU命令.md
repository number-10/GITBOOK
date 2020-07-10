# 内存CPU命令

###查看内存使用情况
```
free -m
free -h
```

-h 指人性化显示，根据大小显示一般为K,M,G。而不是B

###清除缓存

* 清除页面缓存
sync; echo 1 > /proc/sys/vm/drop_caches


* 清除目录项和inode；
sync; echo 2 > /proc/sys/vm/drop_caches



* 清除页面缓存、目录项和节点；
sync; echo 3 > /proc/sys/vm/drop_caches

sync 指清除缓存而不关闭相应程序



### 加内存命令

加后的结果：swap 有值且不为0

```
[root@iz2ze56pkpg6dxap2z3qvzz project]# free -m
              total        used        free      shared  buff/cache   available
Mem:            992         855          63           0          73          30
Swap:          2047         464        1583
```

swapon -s 查看加的分区

```
[root@iz2ze56pkpg6dxap2z3qvzz project]# swapon -s
Filename                                Type            Size    Used    Priority
/opt/swap                               file    2097148 475984  -1
```

* 添加swap文件方式

1、dd命令生成一个固定大小的文件，文件的大小就是添加或扩容swap的大小：

`dd **if**=/dev/zero of=/opt/swap bs=1M count=2048`

2、然后使用mkswap命令将其格式化：

`mkswap /opt/swap`

3、使用swapon命令挂载其：

`swapon /opt/swap`

* 固化 ： 若重启系统，则配置的分区失效。若想固化，要在 `/etc/fstab` 中添加如下一行，使之永久生效

  ```bash
  /opt/swap swap swap defaults 0 0 
  ```

* 删除分区

`swapoff -a`   停止系统正在使用的swap

`swapoff -s`   删除所有的swap

`swapoff /opt/swap` 删除/opt/swap 置换分区
