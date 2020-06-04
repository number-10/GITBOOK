# gitbook FAQ

**Q1:**

    Error: ENOENT: no such file or directory, stat 'E:\GITBOOK\book1\_book\gitbook\gitbook-plugin-sharing\buttons.js'	

**场景:**  启动服务 gitbook serve时候



**A:** 

1、用户目录下找到copyPluginAssets.js文件。

    C:\Users\number10\.gitbook\versions\3.2.3\lib\output\website\copyPluginAssets.js
2、**将所有的 confirm: true 改为 confirm: false **



**分析:**  是一个自带bug（Vesion：3.2.3）



**Q2:_book下html无法跳转问题**

**A1:**  切换gitboob serve 版本

```
gitbook ls-remote
gitbook fetch 低版本
```



**A2:**更改**_book->gitbook->theme.js****文件

* 找到下面的代码搜索  `if(m)for(n.handler` 

* 将`if(m)`改成`if(false)`  保存