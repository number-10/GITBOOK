# Idea maven dependencies红线问题

引言：网上抄来抄去，只解其表，无归纳总结。感觉缺个 灰机站点模式。



**正常情况**：这五个文件是必有的，没有就是没有成功导入jar

_remote.repositories

xxx.jar

xxx.jar.sha1

xxx.pom

xxx.pom.sha1



![image-20210315113447361](E:\GITBOOK\book1\Chapter12：maven\assets\image-20210315113447361.png)

**原因筛选：** 若maven仓库目录有这五个文件，就是原因1；无这五个文件就是原因1、2



## 根据原因解决



### 1、缓存、卡住 问题

* 需要到pom文件 删除引用，再重新引用 jar包
* 到maven仓库删除对应jar 包重新导入
* 到maven仓库对应jar目录lastupdate删除
* idea 工具里 刷新。invalidate;   lifecycle clean,install;    reimport



## 2、 jar包不对

* 检查mavenjar地址，版本是否正确，改正。https://mvnrepository.com/

* 检查maven下载地址是否正确，私网等
* 

## 3、多个项目配置相关

**现状：** 常见于多个微服务，主项目里发现了jar包无法导入问题



**解决：** **idea新窗口打开jar包对应的子微服务/子项目**，这时候子项目会下载对应jar包，再打开主项目也ok。

（如果遇到红线，很久都没解决，网上各种方案都不行，一般就是这个原因）