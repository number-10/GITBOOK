# transacion

# 事务

## 基本概念

* **现实中基本概念**

  做一件事。

* **专业概念**

  * 概念

    > 事务（Transaction），一般是指要做的或所做的事情。在计算机[术语](https://baike.baidu.com/item/术语)中是指访问并可能更新数据库中各种[数据项](https://baike.baidu.com/item/数据项/3227309)的一个程序[执行单元](https://baike.baidu.com/item/执行单元/22689638)(unit)。事务通常由[高级数据库](https://baike.baidu.com/item/高级数据库/1439366)操纵语言或编程语言（如SQL，C++或Java）书写的[用户程序](https://baike.baidu.com/item/用户程序/7450916)的执行所引起，并用形如**begin transaction**和**end tran saction**语句（或[函数调用](https://baike.baidu.com/item/函数调用/4127405)）来界定。事务由事务开始(**begin transaction**)和事务结束(**end transaction**)之间执行的全体操作组成。 [百度百科]

  * 具备特性

    **原子性atomicity：**  许多步骤要么都ok，要么都不OK。==》既然是一件事，那要么做完了，要么没做完

    **隔离性isolation：**  当前事务不影响其它事务             ==》做一件事的过程种，不能影响其它事

    **持久性durability：**   落库后数据能保存，应用崩溃，数据库重启数据也在 。=》物质不会凭空消失，反有过，必有痕

    **一致性consistency：**   ==》账目守恒（落库后，落到硬盘上），能量守恒，依赖前3个。

##  隔离级别



#### 读未提交

> 某个事务未提交前，其修改的数据对其他事务可见,若此时回滚。会造成脏读。当然不可重复读现象/重复读数据不一样，幻读都有。



#### 读已提交

> 当前事务A写数据a，事务A提交后，其它事务才可见**写后**的数据。
>
> 事务A未提交。新增数据a,其它数据不可见；修改数据A,其它事务能读到**写前**的数据，此时事务A提交了，能读到**写后**的数据。会造成写前写后数据不一致，即不可重复读现象/重复度数据不一致现象。



#### 可重复读

> 一个事务内多次读取一个数据一直保存不变，除非自己事务内修改。
>
> 事务A 查询r数据，事务B修改r数据提交。事务A的s数据仍然不变，多次读都是一样。
>
> 事务A 更新r数据，未提交。事务B更新r数据会卡住，等待事务A释放资源。
>
> 可重复读增加了数据的安全性，但是针对新增操作，依然存在幻读问题。



#### 序列化



### 现象及原理

####  脏读

* 应避免，事务A未提交，B读取A操作修改的数据。这个数据就是脏数据，依据脏数据所做的操作很有可能是不正确的。

#### 不可重复读

> 一个事物内两次连续读到的数据是不一样的，这种情况被称为是不可重复读。
>
> 事务A未提交。修改数据a,其它事a务能读到**写前**的数据a-ago，此时事务A提交了，能读到**写后**的数据a-after。**会造成写前写后数据不一致**



#### 幻读

官网定义：

```
The so-called phantom problem occurs within a transaction when the same query produces different sets of rows at different times.

 For example, if a [SELECT] is executed twice, but returns a row the second time that was not returned the first time, the row is a “phantom” row.
```

同一事务2次查询，并不是简单的查询，当前读？，而是数据库实际的数据。



>  隔离级别不高的情况下，对数据加锁或其他。而不是表，事务A操作表的数据，表的其它数据可**被其它事务增删**，造成幻读。



#### 丢失更新 //todo



## 传播级别

**PROPAGATION**  英[ˌprɒpə'ɡeɪʃ(ə)n]   传播

**MANDATORY**  英[ˈmændətəri]  强制的; 法定的; 义务的;

**NESTED**  英[ˈnestɪd]  筑巢; 巢居; 嵌套(信息) 

```
public enum Propagation {
    REQUIRED(0),
    SUPPORTS(1),
    MANDATORY(2),
    REQUIRES_NEW(3),
    NOT_SUPPORTED(4),
    NEVER(5),
    NESTED(6);

    private final int value;

    private Propagation(int value) {
        this.value = value;
    }

    public int value() {
        return this.value;
    }
}
```



### 7种介绍

//todo  加入事务，加入的是什么，上下文？还是上文？本质是加入同一个数据库连接，依赖于数据库默认的事务级别？

#### 1、 默认，REQUIRED 。即@Transactional(propagation=Propagation.REQUIRED)

​		常用。一般没加Transactional没加参数就是默认。

​		依赖于数据库的事务级别。

​		注意：如果被调用端发生异常，那么调用端和被调用端事务都将回滚，反之亦然（注：内层被封装捕获异常处理了不抛出异常外层还是回滚）

​        

#### 2、 support 

​	若调用方 有事务，则加入其事务，没有按非事务的执行



#### 3、MANDATORY

​	若调用方/上下方有事务，则加入其事务，没有抛出异常。==》强制调用方必须有事务

​	**适用场景：** 查漏补缺，强行有事务

​						配置该方式的传播级别是有效的控制上下文调用代码遗漏添加事务控制的保证手段。比如一段代码不能单独被调用执行，但是一旦被调用，就必须有						事务包含的情况

#### 4 、request_new

​		若有事务，加入事务，没有则新建。把之前的事务挂起

​        **适用场景** 

#### 5、NOT_SUPPORTED 

​      总是非事务的执行，若调用方存在事务，则挂起之前事务

#### 6、 NEVER

​    总是非事务地执行，如果存在一个活动事务，则抛出异常。

#### 7、**NESTED**  

   嵌套。如果上下文中存在事务，则嵌套事务执行，如果不存在事务，则新建事务（按REQUIRED）

> 上下文中存在事务：

​        外层事务抛出异常回滚，那么内层事务必须回滚；

​        嵌套的内层事务回滚情况下：

​     	*  **1 若被外层捕获抛出/没封装**  外内都回滚  

​     	*  **2若被外层捕获处理/封装了**  外层不回滚，内层回滚。**此时就是和REQUIRED的区别，若都是外层REQUIRED，内层NESTED** ，REQUIRED哪怕封装处理了				内层。外层也会回滚。

​        

外层事务不可回滚内存提交的事务？？？？//todo





### 

