# 分区表（DM7/ORACLE11G）

意义：提供查询，写入效率。表记录数过大进行分区。比如超过1亿，2亿。

分为水平分区和垂直分区(分列字段)。

### 水平分区 有3种（range分区，hash分区，list分区）

##### range分区
* 一般按时间，例子，卡数据量大于2亿，按创建时间，社保数据，按缴纳时间

####### range分区种的写死分区,分区谁固定。一般有maxvalue 。比如分为 20年以前的数据，20年数据，20年以后的数据。
这样划分。分区数是定时的。
**创建分区表** 
```
CREATE TABLE "FAN"."YW_P_CYZXX_FQ"
(
"ID" VARCHAR2(50) NOT NULL,
"STATUS" VARCHAR2(2) NOT NULL,
"SOURCE" VARCHAR2(2),
"CREATE_TIME" TIMESTAMP(0),
"CREATE_USER" VARCHAR2(50),
"BMBM" VARCHAR2(50),
"BMMC" VARCHAR2(200),
"TGRQ" TIMESTAMP(0),
"RWBH" VARCHAR2(50),
"XM" VARCHAR2(80),
"ZJLX" VARCHAR2(20),
"ZJHM" VARCHAR2(30),
"ZW" VARCHAR2(50),
"SYCB" VARCHAR2(100),
"DJ" VARCHAR2(50),
"YXQSRQ" TIMESTAMP(0),
"YXJZRQ" TIMESTAMP(0),
"DJRQ" TIMESTAMP(6) ,
"DJJGQC" VARCHAR2(200),
"TABLE_VERSION_ID" VARCHAR2(50)
) 
PARTITION BY RANGE (DJRQ)
(
	partition YW_P_CYZXX_FQ_2019_BEFORE values less than ( to_date('2019-01-01','yyyy-MM-dd') ),
	
	partition YW_P_CYZXX_FQ_2019 values less than ( to_date('2020-01-01 00:00:00','yyyy-mm-dd hh24:mi:ss') ),
	
	partition YW_P_CYZXX_FQ_2020 values less than ( to_date('2021-01-01 00:00:00','yyyy-mm-dd hh24:mi:ss') ),
	
	partition YW_P_CYZXX_FQ_2020_AFTER values less than ( to_date('2099-01-01 00:00:00','yyyy-mm-dd hh24:mi:ss'))

);
```

```
CREATE TABLE "FAN"."YW_P_CYZXX_FQ_MAX"
(
"ID" VARCHAR2(50) NOT NULL,
"STATUS" VARCHAR2(2) NOT NULL,
"SOURCE" VARCHAR2(2),
"CREATE_TIME" TIMESTAMP(0),
"CREATE_USER" VARCHAR2(50),
"BMBM" VARCHAR2(50),
"BMMC" VARCHAR2(200),
"TGRQ" TIMESTAMP(0),
"RWBH" VARCHAR2(50),
"XM" VARCHAR2(80),
"ZJLX" VARCHAR2(20),
"ZJHM" VARCHAR2(30),
"ZW" VARCHAR2(50),
"SYCB" VARCHAR2(100),
"DJ" VARCHAR2(50),
"YXQSRQ" TIMESTAMP(0),
"YXJZRQ" TIMESTAMP(0),
"DJRQ" TIMESTAMP(6) ,
"DJJGQC" VARCHAR2(200),
"TABLE_VERSION_ID" VARCHAR2(50)
) 
PARTITION BY RANGE (DJRQ)
(
	partition YW_P_CYZXX_FQ_MAX_2019_BEFORE values less than ( to_date('2019-01-01','yyyy-MM-dd') ),
	
	partition YW_P_CYZXX_FQ_MAX_2019 values less than ( to_date('2020-01-01 00:00:00','yyyy-mm-dd hh24:mi:ss') ),
	
	partition YW_P_CYZXX_FQ_MAX_2020 values less than ( to_date('2021-01-01 00:00:00','yyyy-mm-dd hh24:mi:ss') ),
	
	partition YW_P_CYZXX_FQ_MAX_2020_AFTER values less than (MAXVALUE)

);
```
####### 分区随着时间增加。在插入的数据的时候会生成分区。numtoyminterval(1, 'year') 间隔一年。
缺点：新生成的分区表为系统自动生成的表名。//todo  怎么按一定规则生成分区表名
```
CREATE TABLE "FAN"."YW_P_CYZXX_FQ_ADD"
(
"ID" VARCHAR2(50) NOT NULL,
"STATUS" VARCHAR2(2) NOT NULL,
"SOURCE" VARCHAR2(2),
"CREATE_TIME" TIMESTAMP(0),
"CREATE_USER" VARCHAR2(50),
"BMBM" VARCHAR2(50),
"BMMC" VARCHAR2(200),
"TGRQ" TIMESTAMP(0),
"RWBH" VARCHAR2(50),
"XM" VARCHAR2(80),
"ZJLX" VARCHAR2(20),
"ZJHM" VARCHAR2(30),
"ZW" VARCHAR2(50),
"SYCB" VARCHAR2(100),
"DJ" VARCHAR2(50),
"YXQSRQ" TIMESTAMP(0),
"YXJZRQ" TIMESTAMP(0),
"DJRQ" TIMESTAMP(6) ,
"DJJGQC" VARCHAR2(200),
"TABLE_VERSION_ID" VARCHAR2(50)
) 
PARTITION BY RANGE (DJRQ) INTERVAL (numtoyminterval(1, 'year'))
(
	
	
	partition YW_P_CYZXX_FQ_ADD_2019 values less than ( to_date('2020-01-01 00:00:00','yyyy-mm-dd hh24:mi:ss') )


);
```


**联想思考** etl 增量抽取 可采用CDC方式方式， 对源表的每天记录的监测字段形成hash值保存在CDC表其中一列。后续定期抽取比较hash值，若有变化，则抽取等操作。
和 分区表的hash分区

