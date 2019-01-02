title: Git流
author: Asia Cui
tags:
  - Git
categories:
  - Git流
date: 2019-01-09 18:24:00
---
## Git流
   ![](/images/git/Git流.png)

1. 工作区
  进行文档编写的工作目录。

2. 暂存区
  一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。工作区的文档编辑完，使用`git add`就是提交到暂存区。

3. 本地版本仓库
  工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。使用`git commit`把暂存区的文档提交到本地仓库版本。

4. 远程版本仓库

   在线存储文档仓库，本地仓库版本使用`git push`即可提交到远程仓库，在此之前需要有远程仓库账号、设置`SSH key`，然后把本地仓库和远程仓库关联才可提交。


## Git远程仓库

连接远程仓库需要有git账户，并在本地生成公钥，添加到git账户中。

1. 本地生成SSH key

   ```bash
   #替换git账户绑定的邮箱，然后一路回车执行
   ssh-keygen -t rsa -C "youremail@qq.com"
   ```

   执行完毕后会在 `C:\Users\Admin\.ssh` 目录生成三个文件：`id_rsa` `id_rsa.pub` `known_hosts`，把`id_rsa.pub`的内容粘贴到git的ssh key设置中。


2. 测试是否和远程git连通
  ```bash
  #查看当前是否可以和git远程仓库连通,成功会有successful字样
  ssh -T git@github.com
  ```

3. 远程仓库和本地仓库关联

   ```bash
   #替换git仓库的连接地址
   git remote add origin https://gitee.com/asiagod/your.git
   ```

## 本地项目提交远程仓库

1. Git远程创建仓库

   登录Git账户创建仓库。

2. ssh key设置

   生成ssh key并添加到git账户

   ```bash
   # 替换git账户绑定的邮箱，然后一路回车执行
   ssh-keygen -t rsa -C "youremail@qq.com"
   ```

   > 如果本地连接了多个Git远程仓库，如github.com、gitee.com，在生成ssh key的时候会覆盖之前的，所以需要配置多个git账户。具体配置如下

   ```bash
   #全局配置git账户
   git config user.email "youremail@qq.com"
   #使用此命令生成key，注意指定了文件名为id_rsa.another避免和默认生成的id_rsa重名导致覆盖
   ssh-keygen -t rsa -f ~/.ssh/id_rsa.another -C "youremail@qq.com"
   
   #配置所有git账户，在~/.ssh目录创建config文件
   touch ~/.ssh/config
   #在config文件中配置多个git账户信息如下
   Host gitee.com
       IdentityFile ~/.ssh/id_rsa.another
       User 476494273@qq.com
   Host github.com
       IdentityFile ~/.ssh/id_rsa
       User asiacuiyz@gmail.com  
   ```
   参数详解：

   > - Host：远程Git仓库域名
   > - IdentityFile：对应的id_rsa文件（私钥）
   > - User：生成私钥的邮箱或者用户名

3. 远程关联

  ```bash
  # 替换git仓库的连接地址
  git remote add origin https://gitee.com/asiagod/your.git
  ```

4. 本地操作
   进入到要提交项目目录，执行`pull`命令，把仓库中的文件拉取下来，才能执行`push`，直接执行`push`会报错

   ```bash
   git pull --rebase origin master
   ```

   > 注意：第一次必须使用git pull --rebase origin master命令，以后就可以使用git pull命令

5. 项目提交

  ```bash
  #将文件添加到暂存区
  git add <文件名或文件夹>
  #提交暂存区到本地版本库
  git commit -m "commit msg"
  #提交到远程仓库
  git push -u origin master
  
  ```

