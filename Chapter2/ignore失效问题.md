##  ignore文件失效问题。

原因：ignore文件应为未追踪状态（untracked）,如果先add再加入ignore列表是不生效的。此时文件未已追踪状态（tracked）

解决思路：应先把状态置为未追踪状态。

具体方案：

```
git rm --cached java/.idea/workspace.xml
```