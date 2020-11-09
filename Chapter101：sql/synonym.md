# 同义词

### 作用范围： 
oracle,dm7 数据库。mysql无此概念，可用视图

### 意义：
把表的查询权限给另一个用户时候，另一个用户访问需得  模式名.表名 才能访问。若想直接访问，可通过构建同义词来实现


**例子：**

* 创建表A1 的同义词AS
```
create public synonym AS for 表所属模式名.A1

```

* 赋权给READ用户  **是赋予原来的表名A1,而不是同义词名称AS**
```
grant select on A1 to READ;
```

* 收回权限
```
revoke select on A1 from YWKREAD;
```



* 删除公有同义词

```
drop public synonym AS;
```
