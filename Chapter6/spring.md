 # 1-10章有感

spring 是一种思想，主要要控制反转(ioc)和面向切面（aop）.

spring框架有实现ioc的容器和面向切面编程。

springmvc是具体的实现。

出现的原因。很久以前（待定），类等资源依赖于容器工具实现，容器工具的选择不同 如weblogic，标准也不同，往往杂乱不章，高耦合。代表EJB(todo);



**主要思想**

所有的类资源都给IOC容器，本来需要你创建的类，现在你要通过**描述**告诉IOC容器，你要创建什么类。

现实例子：你要做蛋炒饭，然后先买鸡蛋和米，然后打开火，炒菜一系列事情，是你去主动实现。这是主动创建。

​                  你去外卖平台订个蛋炒饭，你只需要选择店家，和饭的规模就行了。你是把需求/描述告诉了外卖平台，店家。他们去完成炒饭的过程。你并没有主动创建。控制权交给了外卖平台。

好处：专业的事情交给专业的人。人只需要做好自己的事情



控制反转原理或依赖于 动态代理和反射技术/思想。动态代理有JDK动态代理和CJLIB.区别（有无接口？）

描述分为三种：按权限级别有限性来排：

1、约定以及自动装配的规则

2、在类上的使用如setter,构造方法，注解

3、xml文件



IOC容器创建类的流程。

初始化->依赖注入

初始化：

获取类的定义（todo）

定位资源

注册资源

依赖注入（原理 反射？todo）：根据描述来注入。**注入的方式**有 setter.构造方法,注解



**注解具体**

@component

@aoutwired



类注入容器里会有一个名称。可通过描述指定（注解@componet(类名称)，xml id=""），如果不指定默认是对应的类 全名称？ +number。

注解可在类上，类的方法上 如数据源配置  DateSouce getSourceDate()，方法的参数上。



描述的用法：一般自己构建开发的类用注解，第三方的用xml.



