# 同义词

### 作用范围： 
oracle,dm7 数据库。mysql无此概念，可用视图

### 意义：
把表的查询权限给另一个用户时候，另一个用户访问需得  模式名.表名 才能访问。若想直接访问，可通过构建同义词来实现


**例子：**

* 创建表A1 的同义词A1
```
create public synonym A1 for 表所属模式名.A1

```

* 赋权给READ用户
```
grant select on A1 to READ;
```

* 收回权限
```
revoke select on A1 from YWKREAD;
```

* 删除共有同义词

```
drop public synonym A1S2;
```
