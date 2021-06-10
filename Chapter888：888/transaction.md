# transacion

[toc]


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

事务之间的传播影响，至少2个

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

**这里的当前，相当于前提里的上文。** 是站在子事务角度考虑。系统配置模式事务（爷爷）-》父事务-》子事务

| 事务传播行为类型             | 说明                                                         |
| :--------------------------- | :----------------------------------------------------------- |
| **PROPAGATION_REQUIRED**     | **如果上文没有事务，就新建一个事务；如果上文有事务中，加入到这个事务中。@Transactional默认配置。** |
| PROPAGATION_SUPPORTS         | 如果上文没有事务，就以非事务方式执行（都无事务，有异常也不回滚）。上文有事务，就加入 |
| PROPAGATION_MANDATORY        | 如果上文没有事务，就抛出异常，若封装就不会影响上文。上文有事务，就加入。 |
| **PROPAGATION_REQUIRES_NEW** | **上文没有事务，现在的新建事务；上文有事务，把上文事务挂起，再新建现在的事务。 上文事务、现在事务互不影响** |
| PROPAGATION_NOT_SUPPORTED    | 以非事务方式执行操作，如果上文存在事务，就把上文的事务挂起。 |
| PROPAGATION_NEVER            | 以非事务方式执行，如果上文存在事务，则抛出异常 。            |
| PROPAGATION_NESTED           | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。 |

####  前提

##### **常识：** 

* **抛出异常必须是throw  new RuntimeException(); 其它的起不到回滚作用，throw  new Exception(); 也不行。**

* **无论何种传播配置，子事务加入（不是嵌套）父事务。 有一方事务回滚，另一方必定回滚，无论有没有封装方法捕获异常。**

  **原理是加入就相当于一个事务。子加入父相当于一个事务方法且是不捕获异常的情况。**
  
* **子事务挂起父事务 =》父子事务无关、无影响**

##### **例子：** 必须是两个类的各一个方法。同一个类的2个方法无效（相当于子方法copy到父方法里，父类事务还是生效的，）。因为代理的原因**//todo**



* **正确例子1:**

 TParent：事务父亲方法  =   VideoServiceImpl里的ServerTransRequest   

 TSon:        事务子方法   =    SysLogServiceImpl里的transLogSon



* **正确例子2（调用同一个类中的方法）。**TParent,TSon在一个类C1中。

TParent中：

```
C1 thisBean = (AliOssService)SpringContextUtils.getBean("C1");
boolean flag  = thisBean.Tson();
```

```
@Component
public class SpringContextUtils implements ApplicationContextAware {
	public static ApplicationContext applicationContext; 

	@Override
	public void setApplicationContext(ApplicationContext applicationContext)
			throws BeansException {
		SpringContextUtils.applicationContext = applicationContext;
	}

	public static Object getBean(String name) {
		return applicationContext.getBean(name);
	}

	public static <T> T getBean(String name, Class<T> requiredType) {
		return applicationContext.getBean(name, requiredType);
	}

	public static boolean containsBean(String name) {
		return applicationContext.containsBean(name);
	}

	public static boolean isSingleton(String name) {
		return applicationContext.isSingleton(name);
	}

	public static Class<? extends Object> getType(String name) {
		return applicationContext.getType(name);
	}

}
```

捕获后再 抛出异常：

```java
...
}catch (Exception e){

   e.printStackTrace();
   //法一
   throw new RuntimeException();
   //法二
   TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();

}
```



**本文无特殊说明，则 子事务方法是封装在父事务方法里的。即异常捕获不抛出。**  因为，子事务没被封装，肯定会抛出到父事务方法，相当于父事务方法本身有异常抛出。

**TParent:**   **父事务、** **即上文事务、**  **即调用方**

```
@Service("videoService")
public class VideoServiceImpl extends ServiceImpl<VideoDao, VideoEntity> implements VideoService {


    @Resource
    SysLogService sysLogService;
    
    
    @Override
    @Transactional(isolation = Isolation.DEFAULT,propagation = Propagation.REQUIRED)  //此事务注解起作用
    public void ServerTransRequest()  throws Exception{



        VideoEntity video = new VideoEntity();
        video.setVideoName(UUID.randomUUID().toString());
        videoDao.insert(video);


        try {
             //throw  new RuntimeException();
            
           //实际起作用的是下面的 “ sysLogService.transLogSon();//这里面是一个独立的事务配置” ;等同于直接写sysLogService.transLogSon();	 
            transLogSon();        
            
        
        } catch (Exception e) {
            e.printStackTrace();
        }


        //throw  new RuntimeException();  //异常1001 TParent 异常  

    }

    @Transactional(propagation = Propagation.NEVER)  // 此事务注解不起作用
    public void transLogSon() throws Exception {
        sysLogService.transLogSon();//这里面是一个独立的事务配置，里的事务注解起作用，SysLogServiceImpl里
        throw  new RuntimeException();  //此异常也不起作用,当然能到父事务方法里且被抛出肯定有作用。(相对于父事务方法里本身异常了)
    }


    
```



**TSon:**   **子事务、**  **即现在的事务、** **即被调用方**

```
@Service("sysLogService")
public class SysLogServiceImpl extends ServiceImpl<SysLogDao, SysLogEntity> implements SysLogService {
    @Override
    @Transactional(isolation = Isolation.DEFAULT ,propagation = Propagation.REQUIRES_NEW)  //事务注解TSon 生效
    public void transLogSon() throws Exception {
        SysLogEntity sysLogEntity = new SysLogEntity();
        sysLogEntity.setUsername(UUID.randomUUID().toString());
        sysLogEntity.setTime(System.currentTimeMillis());
        this.save(sysLogEntity);
       // int i =1/0;
        //throw  new RuntimeException();

    }
```



自我理解，按 子事务为现在的事务；上文事务为父事务

#### 传播级别

#### 1、 默认，REQUIRED 。即@Transactional(isolation = Isolation.DEFAULT ,propagation = Propagation.REQUIRED)

​		常用。一般没加Transactional没加参数就是默认此级别。

​      上文无事务：新建事务；上文有事务，加入上文事务。==》**强调  加个事务一般情况（没有就新建，有就加入）**

​		注意：加入上文事务情况下：如果子事务发生回滚，父子事务都将回滚，反之亦然

**注：** **事务方法内层异常抛出到外层,外层事务捕获异常不在外层抛出，还是会全部回滚！(外层是通过代理调用内层,不是同一类上下调用)**；

​		**凡是子事务加入（不是嵌套到）父事务都是如此，比如support、MANDATORY的加入父事务情况 。**

​		**REQUIRED**  和 **NESTED（嵌套）**  的区别就在此。**NESTED**   子事务回滚，父事务不会回滚。

例子：

图A里面外层调用了  代理的事务方法 transLogSon(); 若transLogSon() 里有异常抛出，整体都会回滚（包括videoDao.insert(video)）。

图B里面的外层事务方法自己内部异常，不会回滚。

图C里面的内层事务方法自己内部异常，不会回滚，且不影响上层事务。

**图A:**

![image-20210610113036410](E:\GITBOOK\book1\Chapter888：888\assets\image-20210610113036410.png)

​		           

**图B:**

![image-20210610134528858](E:\GITBOOK\book1\Chapter888：888\assets\image-20210610134528858.png)





**图C**

![image-20210610112833883](E:\GITBOOK\book1\Chapter888：888\assets\image-20210610112833883.png)



​       

​        

#### 2、 support   ==》强调 子方法跟随性 

​	若上文事务，则加入其事务，没有按非事务的执行



#### 3、MANDATORY

​	若调用方/上下方有事务，则加入其事务，没有抛出异常。==》**强调  必须上下文都有事务，否则异常**

​	**适用场景：** 查漏补缺，强行有事务

​						配置该方式的传播级别是有效的控制上下文调用代码遗漏添加事务控制的保证手段。比如一段代码不能单独被调用执行，但是一旦被调用，就必须有						事务包含的情况



#### 4 、request_new  ==》强调   新建事务，保持独立性

​        上文有事务，挂起上文事务，新建当前事务。

​        上文无事务，新建事务

​         

​       适用场景：两个事务互不影响；父事务回滚，不影响子事务。子事务回滚，不影响父事务。比如记录操作后，记录日志。操作和记录日志互不影响。

​      上文回归

​       

#### 5、NOT_SUPPORTED 

​      总是非事务的执行，若调用方存在事务，则挂起之前事务 ==> **强调 现在非事务，且上下文无关系/不跟随**

#### 6、 NEVER

​    总是非事务地执行，如果存在一个活动事务，则抛出异常。 ==》**强调 上下文无事务，否则异常**

#### 7、**NESTED**   嵌套

   嵌套。如果上下文中存在事务，则嵌套事务执行，如果不存在事务，则新建事务（按REQUIRED） ==》 **强调 子事务嵌套到父事务，不是加入**

  上文中存在事务：

​      外层事务回滚，那么内层事务必须回滚；

​       **内存回滚，外层不回滚（这就是嵌套和REQUIRED 的区别）**。当然 父事务回滚，子事务必回滚。

   上文没有事务：

​      新建事务。

​     	



 ### 多数据源情况呢？分布式情况呢？ //todo

