**Q:**push commit的报错:

​       Push failed
​		Ssh: Could not resolve hostname github.com: Name or service not known
​		Could not read from remote repository.
​		Please make sure you have the correct access rights
​		and the repository exists.



**译文：**

推送失败
ssh：无法解析主机名github.com：名称或服务未知
无法从远程存储库读取。
�请确保您具有正确的访问权限
且存储库存在。

**分析：** 没有连接上git仓库，请确认权限配置或连接是否ok



**确认问题：**  在cmd窗口执行 ```ssh -T git@github.com``` 检测连接问题

```

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions for 'C:\\Users\\number10/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "C:\\Users\\number10/.ssh/id_rsa": bad permissions
git@github.com: Permission denied (publickey)
```

译文：

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@ @@@@@@@@@@
//警告：未保护的私钥文件！ @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@ @@@@@@@@@@
'C：\\ Users \\ number10 / .ssh / id_rsa'的权限太开放。
要求他人不能访问您的私钥文件。
此私钥将被忽略。
加载密钥“ C：\\ Users \\ number10 / .ssh / id_rsa”：权限错误
git@github.com：权限被拒绝（公钥）



**分析：** 提示ssh配置的私钥权限太开放，要求其他人不能访问。 那就按要求的改为只有当前用户能访问



**A:** **找到对应id_rsa文件。修改其权限。**

  **属性->安全->高级： 1禁用继承 2权限条目：除了本用户，其它用户、组删除**



**测试是否ok:**  在cmd再次执行  ```ssh -T git@github.com``` 。若返回

Hi number-10! You've successfully authenticated, but GitHub does not provide shell access.即代表解决成功