# ignore文件

### 一、 ignore文件配置 。再主目录下建文件 .gitignore
* 配置规则：https://git-scm.com/docs/gitignore

例子：
* 官方：https://github.com/github/gitignore
* 本人gitbook .gitignore 内容

```
# dependencies  npm包文件
/node_modules

#serve
/_book

#idea
/.idea

```

### 二、 ignore文件失效问题。

原因：ignore文件应为未追踪状态（untracked）,如果先add再加入ignore列表是不生效的。此时文件未已追踪状态（tracked）

解决思路：应先把状态置为未追踪状态。

具体方案：

文件

```
git rm --cached <file>  
```



文件夹

法一：

 git rm --cached  -r 文件夹



法二:全部移除，再全部加上

 `git rm -r --cached .`

`git add .`