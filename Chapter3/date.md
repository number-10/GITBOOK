# linux时间: date

```
    # date "+%Y%m%d%H%M%S"
    20200722 164106
```
%y  20

%H=%k  16



获取当前时间，并赋给变量，当作文件名的参数


```
filenmae=`date +%Y%m%d%H%M`
mkdir ${filenmae}
```

每分钟保存top脚本
```
#!/bin/bash
filename=`date "+%Y%m%d%H%M" `
mkdir /data/slog/${filename}.log
echo ${filename}
top -d 1 -n 1 >>/data/slog/${filename}.log

```
*  filename= 后面的符合 ` 处于键盘左上角。不是单引号

【`】是国际标准万国码的全形抑音符（全宽重音符号）
英文名为【FULLWIDTH GRAVE ACCENT】。位于万国码uFF40。