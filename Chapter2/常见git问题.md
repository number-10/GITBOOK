### P1

* **问题场景** 

  执行此命令  $ git push origin develop:develop

```
 To https://gitee.com/xxx.git
 ! [rejected]        develop -> develop (non-fast-forward)
error: failed to push some refs to 'https://gitee.com/xxx.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

对应中文释义：

```
错误：无法将某些引用推送到“ https://gitee.com/xxx.git”
提示：由于您当前分支的提示位于后面，因此更新被拒绝
提示：它的远程副本。 集成远程更改（例如
提示：“ git pull ...”），然后再次按下。
提示：有关详细信息，请参见“ git push --help”中的“关于快进的注意事项”。
```



**分析：**

* 问题场景：

  提交本地分支到远程，**但远程已有代码**，大概率是在本地仓库第一次连接远程仓库。
  
  本质：两个仓库不一致，无相同的祖先，无一个共同的commit等。git 程序无法确认两个仓库相关。
  
  

**解决方案：**

* **法一**【建议使用】：**先获取远程分支代码，再提交**

   **git pull origin develop --allow-unrelated-histories**

```
git pull origin develop --allow-unrelated-histories
From https://gitee.com/xxx
 * branch            develop    -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 README.en.md | 36 ++++++++++++++++++++++++++++++++++++
 README.md    | 37 +++++++++++++++++++++++++++++++++++++
 2 files changed, 73 insertions(+)
 create mode 100644 README.en.md
 create mode 100644 README.md
```



* **法二**【不建议使用】：**强制提交**，会覆盖远程仓库代码，仅可在远程代码无重要内容/空/初次建立连接的时候使用。

  **git push -f origin develop**



* **土法：**

  把远程代码 clone下来, 此时仓库和远程一致。再把自己的文件加进去，提交。

  

**注：若法一使用 git pull origin develop**  可能会报

```
$ git pull origin develop
From https://gitee.com/xxx

- branch            develop    -> FETCH_HEAD
  fatal: refusing to merge unrelated histories
```

对应中文释义：

```
-分支开发-> FETCH_HEAD
   致命的：拒绝合并无关的历史
```





### <u>知识点：</u>  **--allow-unrelated-histories**

对应中文释义：允许无关的历史记录