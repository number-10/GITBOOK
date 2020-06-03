# gitbook FAQ

**Q:**

    Error: ENOENT: no such file or directory, stat 'E:\GITBOOK\book1\_book\gitbook\gitbook-plugin-sharing\buttons.js'	

**场景:**  启动服务 gitbook serve时候



**A:** 

1、用户目录下找到copyPluginAssets.js文件。

    C:\Users\number10\.gitbook\versions\3.2.3\lib\output\website\copyPluginAssets.js
2、**将所有的 confirm: true 改为 confirm: false **







**分析:**  是一个自带bug（Vesion：3.2.3）