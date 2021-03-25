# 第一节：git安装

## 一、 git概念，好处，

[官网](https://git-scm.com/)

Git是一个[免费的开源](https://git-scm.com/about/free-and-open-source) 分布式版本控制系统，旨在快速高效地处理从小型到大型项目的所有内容。

Git[易于学习](https://git-scm.com/doc)， [占地面积小，具有闪电般的快速性能](https://git-scm.com/about/small-and-fast)。它具有诸如Subversion，CVS，Perforce和ClearCase之类的SCM工具，具有[廉价的本地分支](https://git-scm.com/about/branching-and-merging)，便捷的[暂存区域](https://git-scm.com/about/staging-area)和 [多个工作流等功能](https://git-scm.com/about/distributed)。



## 二、安装

https://git-scm.com/download



### 1、window

https://git-scm.com/download/win



###  2、linux CentOs7 安装git

https://git-scm.com/download/linux

```
# yum install git
```

centos7默认版本是
`git version 1.8.3.1`

写文档时候最新版本是2.27.0 

若想安装新版本 **todo**；先删除 yum remove git；再寻找 yum search git




#### 查看安装目录

* windows系统
```
where  git
```



## 三、git参数配置

系统，用户，仓库。优先级 仓库>用户>系统

**参数查看**

* **系统：**

   ```git config --system -l```

* **用户**

  ` git config --global -l`

* **单个项目仓库**

  `git config --local -l`