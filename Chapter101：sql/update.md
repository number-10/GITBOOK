# 更新语句

**两边关联更新  表2的字段更新到表1中**：

**初始数据：**

**表user1**

| ID   | NAME | SEX  |
| ---- | ---- | ---- |
| 1    | ZS   | 男   |
| 2    | LS   | 男   |
| 10   | WU   | 女   |

**表user2**

| ID   | NAM  | SEX  |
| :--- | ---- | ---- |
| 1    | 张三 | 男   |
| 2    | 李四 | 男   |
| 3    | 王五 | 男   |
| 4    | 赵六 | 男   |





```sql
update  u1 set u1.name=u2.name,u1.sex=u2.set 
from user1 u1,user2 u2 where u1.id=u2.id
```

```
update  u1 set u1.name=u2.name,u1.sex=u2.set 
from user1 u1 inner join user2 u2 on u1.id=u2.id
```

**注意：一定要inner join 不能left join 否则会出现 表1 id=10 的记录name为空的现象。right join 也行。**







```
update user1 u1 set 
u1.name=(select name from user2 u2 where u2.id=u1.id) ,
u1.sex=(select sex from user2 u2 where u2.id=u1.id) 
where exists (select 1 from user2 uu2 where uu2.id=u1.id)
```
or
```
update user1 u1 set 
(select u1.name,u1.sex) =(select name,sex from user2 u2 where u2.id=u1.id) 
where exists (select 1 from user2 uu2 where uu2.id=u1.id)
```



**注意：where exists (select 1 from user2 uu2 where uu2.id=u1.id)**  一定要加。**否则若匹配列匹配不到。如表1 的id=10。则会出现表1的name更新后为空的情况**



