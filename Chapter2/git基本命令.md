# git基本命令

**原则: git push 和 pull都是对应本机来说.所以前面为 git push/pull  <远程主机名>**

**后面为 <源>:<目的>**



## git push

```javascript
$ git push <远程主机名>   <本地分支名>:<远程分支名>
```







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