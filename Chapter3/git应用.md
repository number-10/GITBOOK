### git切换源（检查项目后切换到其它源）

**用途：**copy项目

**步骤：**

**1、检出项目** 

 略。

**2、删除原有远程源**

```
git remote remove origin 
```

注：origin 为远程源别名。可用 $ git remote show 查看。

**3、添加远程源**

```
$ git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

**更改远程仓库的url**  

 `git remote set-url` 命令将远程的 URL 从 HTTPS 更改为 SSH。 [官网链接](https://docs.github.com/cn/github/using-git/changing-a-remotes-url#switching-remote-urls-from-https-to-ssh)

```shell
$ git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

**4、本地项目数据上传到新建的远程仓库分支，且建立联系**

```
$ git push -u origin master
Enumerating objects: 1976, done.
Counting objects: 100% (1976/1976), done.
Delta compression using up to 12 threads
Compressing objects: 100% (793/793), done.
Writing objects: 100% (1976/1976), 11.59 MiB | 247.28 MiB/s, done.
Total 1976 (delta 1008), reused 1962 (delta 1004)
remote: Resolving deltas: 100% (1008/1008), done.
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/number-10/RH/pull/new/master
remote:
To https://github.com/number-10/RH.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

```

注：若远程仓库无对应分支，则会自动创建。比如远程分支master.

$ git push <远程主机名>   <本地分支名>:<远程分支名>     

“<本地分支名>:”可省略，默认本地当前分支。

**5，去除之前的历史提交记录。**

**分支法：** 

* **新建并切换到 orphan 类型分支**

  ```
  git checkout --orphan Lor1
  ```

​       copy当前分支到 Lor1分支，但不会copy 提交信息等。此时看不到此分支。commit后才能

​      orphan（孤儿） 类型分支：没有父结点，没有历史提交记录

* **提交已追踪的文件**

  ```
  git commit -am 'init'
  ```

​       git commit m 提交暂存区文件

* **上传文件到新分支，并建立联系**

  ```
  git push -u origin Lor1
  ```

  此时新分支Lor1为只有一个提交记录（init）的分支。

  后续可删除其它分支，重命名分支等

  Q1：

  error: src refspec <远程分支>does not match any
  error: failed to push some refs to <远程git地址>

​       A1：  git push -u origin  <当前分支>:<远程分支>

