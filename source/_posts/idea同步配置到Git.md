title: idea同步配置到Git
author: Asia Cui
tags:
  - idea
categories:
  - 同步配置
date: 2019-01-23 09:58:00
---


## 软件安装

- IntelliJ IDEA
- [ToolBox](https://www.jetbrains.com/toolbox/app/?fromMenu)
- Git

## 同步步骤

同步之前，现在GitHub上创建存储配置仓库，创建完仓库设置`token`，设置步骤是GitHub的`Settings`->`Developer settings`->`Personal access tokens`->`Generate new token`，点击创建`token`，如下图所示：

![1548208510812](/images/idea/1548208510812.png)

进入`idea`界面，点击`File`->`Settings Repositorys`，输入`Git`的`URL`，点击`Overwrite Remote`即可。