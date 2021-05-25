# 场景1： 从服务器端拉取另一个分支的代码到本地。



比如:远程是origin。有分支master/develop/test;

idea拉取默认本地是master; 怎么让本地也有3个分支分别对应远程分支。


* 法一：--track

  ```
  $ git checkout --track origin/develop
  ```

  

* 法二：简略版，适用于本地没有develop分支

  ```
  $ git checkout develop
  ```

  让本地dev对应origin:develop：```git checkout -b dev origin/develop```

  

* 法三：老实人  //todo

  * 第一步，新建本地空白分支develop,并提交到暂存区 

    ```
    $ git checkout --orphan develop
    $ git commit -am 'init'
    ```

    

  * 第二步，本地develop和远程devlelop建立联系

    ```git branch  dev --orphan
    
    ```





**注：法一、二、三  前提是已有远程分支origin/develop的列表,即git branch -a 红色部分有   remotes/origin/test。否则远程仓库上有 test分支 也无用。**

法一会报错误：fatal: 'origin/test' is not a commit and a branch 'test' cannot be created from it

法二会报错误：error: pathspec 'test' did not match any file(s) known to git

原因：//todo

解决方案：

* 先获取所有分支

​    ```git fetch```

若git pull 有问题说明没建立连接。可用 ```git branch -u origin/test test```



# 场景2：本地dev分支新代码，提交到远程orgin/test分支

法一：

* 本地dev提交到远程分支origin:tmp。
* 拉取远程test分支代码。
* 合并origin:tmp 代码到本地test
* 提交本地test代码到远程origin/test分支（//若在此条命令之前 删除origin:dev2会怎么样）



法二：

* 拉取远程test分支代码。

  ```git checkout --track origin/test```

* 本地分支dev合并到本地分支test（merge a to b 。不会改变b原有内容，会增加b没有而a有的内容）

  * 切换到test分支：       ```git checkout test```
  * 合并dev分支到test： ```git merge dev```

* 提交本地test代码到远程origin/test分支

  * 先拉取   ```git pull origin test```

  * 再提交   ```git push origin test```

  

  >tip:
  >
  >git push <远程主机名>   <本地分支名>:<远程分支名>
  >
  ><本地分支名>: 可省略，默认本地当前分支。



**参考**：

[https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF](https://git-scm.com/book/zh/v2/Git-分支-远程分支)





## 撤消才提交的本地commit

git reset --soft HEAD^



## 拉取远程代码冲突

git stash push -- 入栈，即推送本地改动内容到栈，把本地改动存起来

git pull -- 拉取服务端代码

git stash pop -- 出栈，把本地改动的代码还原
