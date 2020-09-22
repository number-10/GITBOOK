# linux定时任务: crontab

### 命令
crontab -l 显示定时列表
crontab -e 编辑
crontab -r 删除

grep top.sh /var/log/cron* 查询执行脚本日志，top.sh 为脚本名称



每月1,2,3号 凌晨2点执行脚本： 周期(0 2 1,2,3 * * )   脚本(/etc/cron.d/changeFile777.sh)。

```shell
# crontab -e 
```
若之前没有 则会自动创建，然后用vi相关命令 添加任务。（周期 脚本 写在第一行，第二行等）
```
0 2 1,2,3 * *  /etc/cron.d/changeFile777.sh
```


定期更改某类文件权限脚本 changeFile777.sh
```
#/bin/bash
chmod -R 777 /data/SZGJJ/RECV_HERE/gjj*
```



##### 周期 5个。

分 时 天 月 周

* 直接写数字代表更大范围里的当前单位值。 比如上文的0代表每小时的第0分。
2 代表 每天的第2点。 1,2,3 代表每月的第1，2，3号。

**操作符有**

\* 取值范围内的所有数字
/ 每过多少个数字
\- 从X到Z
, 散列数字



实例1：每1分钟执行一次myCommand
```
* * * * * myCommand
```


实例2：每小时的第3和第15分钟执
```
3,15 * * * * myCommand实
```

例3：在上午8点到11点的第3和第15分钟执行
```
3,15 8-11 * * * myCommand
```
实例4：每隔两天的上午8点到11点的第3和第15分钟执行
```
3,15 8-11 */2  *  * myCommand
```

实例5：每周一上午8点到11点的第3和第15分钟执行
```
3,15 8-11 * * 1 myCommand
```

实例6：每晚的21:30重启smb
```
30 21 * * * /etc/init.d/smb restart
```

实例7：每月1、10、22日的4 : 45重启smb
```
45 4 1,10,22 * * /etc/init.d/smb restart
```

实例8：每周六、周日的1 : 10重启smb
```
10 1 * * 6,0 /etc/init.d/smb restart
```

实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb
```
0,30 18-23 * * * /etc/init.d/smb restart
```

实例10：每星期六的晚上11 : 00 pm重启smb
```
0 23 * * 6 /etc/init.d/smb restart
```

实例11：每一小时重启smb
```
* */1 * * * /etc/init.d/smb restart
```

实例12：晚上11点到早上7点之间，每隔一小时重启smb
```
* 23-7/1 * * * /etc/init.d/smb restart
```

例子来源：https://www.runoob.com/w3cnote/linux-crontab-tasks.html





案例 研究 https://www.cnblogs.com/wangqiguo/p/5399227.html