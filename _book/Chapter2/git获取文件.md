# 第一节：强制获取remote文件 本地文件删除后

删除本地文件后，想从远程仓库中从新Pull最新版文件。

Git提示：All files are up-to-date，但未从服务器上获取到已删除的文件

原因：当前本地库处于另一个分支中，需将本分支发Head重置至master. **待确定**

```basic
git checkout master //危险操作 文件做的任何修改都会消失 - 你只是拷贝远程文件来覆盖它
git reset --hard
```

- git 强行pull并覆盖本地文件

```basic
git fetch --all 
git reset --hard origin/master
git pull
```

