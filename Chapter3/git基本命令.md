

# git基本命令



## git commit 

git commit -m "redis"  ./Redis.md

提交暂存区 特定文件





***

**原则: git push 和 pull都是对应本机来说.所以前面为 git push/pull  <远程主机名>**

**后面为 <源分支>:<目的分支>**



## git push

```javascript
$ git push <远程主机名>   <本地分支名>:<远程分支名>
```



  <本地分支名>:   可省略，代表当前分支。



## git pull

git pull 操作就可以从远程库中获取某个分支的更新，再与本地指定的分支进行自动merge(即使本地不存在这个分支)

完整格式是：

```
$ git pull <远程库名>   <远程分支名>:<本地分支名>
```

比如，取回远程库中的online分支，与本地的online分支进行merge，要写成：

```
git pull origin online:online
```

如果是要与本地当前分支merge，则冒号后面的<本地分支名>可以不写

```
git pull origin online
```

通常，git会将本地库分支与远程分支之间建立一种追踪关系。比如，在git clone的时候，所有本地分支默认与远程库的同名分支建立追踪关系。也就是说，本地的master分支自动追踪origin/master分支。因此，如果当前处于本地online分支上，并且本地online分支与远程的online分支有追踪关系，那么远程的分支名可以省略



* **git pull的默认行为是git fetch + git merge**



### git clone

**A:** git clone -b master git@github.com:number-10/Library-sframe-sc.git

**B:** git clone -b master git@github.com:number-10/Library-sframe-sc.git     ./

区别。

如何去除主目录 采用方法B



### 查看分支&切换&新建

#### 1、查看

##### 本地分支

查看分支列表 *代表当前检出/选中的分支

```
git branch
* master
```

查看分支最后一次提交

```
$ git branch -v
* master 9c0d0dc pom
```

 

##### 远程分支

查看所有远程分支

```
$ git branch  -r
  origin/HEAD -> origin/master
  origin/master
```

 `git remote show <remote>` 获得远程分支的更多信息

```
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:number-10/Library-sframe-sc.git
  Push  URL: git@github.com:number-10/Library-sframe-sc.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (local out of date)



```



##### 本地分支&远程分支一起查看

```
$ git branch -vv
* master 9c0d0dc [origin/master] pom

$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master

$ git branch -avv
* master                9c0d0dc [origin/master] pom
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master 9c0d0dc pom
```

git branch -vv：   master 为本地分支， [origin/master] 远程分支

git branch -a：  master 为本地分支， 红色代表服务器情况 remote/origin/mastet远程分支。

remotes/origin/HEAD -> origin/master 应改表示 服务器当前指针指向 origin/master

**一般用git branch -vv** 清晰，简化







#### 2、切换分支

git checkout  devlop



#### 3、新建

**本地分支**

``` $ git branch newLocalBranch```

新建的分支和本地库一致



**远程分支**

```
$ git push origin newLocalBranch:newRemoteBranch
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'newRemoteBranch' on GitHub by visiting:
remote:      https://github.com/number-10/Library-sframe-sc/pull/new/newRemoteBranch
remote:
To github.com:number-10/Library-sframe-sc.git
 * [new branch]      newLocalBranch -> newRemoteBranch

```



**本地分支、远程分支 建立联系：**

```
$ git push -u origin master
```





##### 4、删除

删除本地分支

git branch -D <分支名>



删除远程分支

git push origin  :<远程分支>



推送空到远程库分支，空替换远程库分支

```
$ git push origin :develop
To https://github.com/xxx.git
 - [deleted]         develop


```



