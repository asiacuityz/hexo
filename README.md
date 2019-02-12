title: GitHub搭建Hexo
author: Asia Cui
top: true
tags:
  - 安装教程
categories:
  - GitHub搭建Hexo
date: 2018-12-29 13:24:00
---
### 安装简介

- Git
- Node.js
- hexo-cli



### 安装Git

Git 各平台安装包下载地址为：<http://git-scm.com/downloads>

### 安装Node.js

Node.js 安装包及源码下载地址为：<https://nodejs.org/en/download/>

安装完毕测试：

```bash
$ node -v
```

### 安装hexo-cli客户端

`hexo-cli` 客户端可以执行 `hexo` 命令。

```bash
$ npm install hexo-cli -g
```

### 创建博客目录

创建一个 `hexo` 文件夹，进入到该目录中初始化。
```bash
#初始化
$ hexo init
#清理项目
$ hexo clean
#生成文章
$ hexo g
#启动hexo
$ hexo s
```
### 博客安装主题
进入到 `hexo` 的 `theme` 目录中，使用 `gitbash` 执行命令
```bash
#git下载主题
$ git clone https://github.com/blinkfox/hexo-theme-matery.git
```
下载完毕后修改 `hexo` 目录下的  `_config.yml` 配置，把 `theme` 的值修改为主题文件夹的名称

```yaml
theme: hexo-theme-matery
```
### 主题配置
代码高亮安装[hexo-prism-plugin]( https://github.com/ele828/hexo-prism-plugin)插件

```bash
$ npm i -S hexo-prism-plugin
```

然后修改`hexo` 目录下的  `_config.yml`的值`highlight.enable`为`false`，并添加`prism`插件如下：
```yaml
#修改为false
highlight:
  enable: false
#添加插件配置
prism_plugin:
  mode: preprocess
  theme: base16-ateliersulphurpool.light #代码高亮主题
  line_number: false    # default false
  custom_css:
```

其中支持的代码高亮主题有如下几种：
```yaml
theme:
  - default
  - coy
  - dark
  - funky
  - okaidia
  - solarizedlight
  - tomorrow
  - twilight
  - atom-dark
  - base16-ateliersulphurpool.light
  - cb
  - duotone-dark
  - duotone-earth
  - duotone-forest
  - duotone-light
  - duotone-sea
  - duotone-space
  - ghcolors
  - hopscotch
  - pojoaque
  - vs
  - xonokai
```
### 安装搜索插件
使用Hexo插件 `hexo-generator-search` 搜索内容，安装命令如下：
```bash
$ npm install hexo-generator-search --save
```
然后在 `hexo` 目录下的 `_config.yml` 添加配置，如下所示：
```yaml
search:
  path: search.xml
  field: post
```
### 安装Hexo-admin插件
本地安装完，只能在本地使用，上传github访问不到
```bash
#安装
$ npm install --save hexo-admin
#启动
$ hexo server -d
```
访问 `http://localhost:4000/admin`即可。
### GitHub发布
#### 创建仓库
在github创建一个仓库 `asiacuityz/asiacuityz.github.io`，注意 `/` 前后的`asiacuityz` 一定要保持一致，加个后缀`.github.io`即可。
#### SSH生成添加
[GithubSSH文档](https://help.github.com/articles/connecting-to-github-with-ssh/)
根据邮箱生成SSH，然后会提示输入密码之类的操作，具体命令如下：
```bash
#生成SSH
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
#添加SSH
$ ssh-add ~/.ssh/id_rsa
```
执行完命令，窗口会显示key文件生成目录在`.ssh`下，然后把文件`id_rsa.pub`里的内容添加到GitHub账户中。添加完毕之后在`_config.yml`配置如下：
```yaml
deploy:
  type: git
  repository: git@github.com:asiacuityz/asiacuityz.github.io.git
  branch: master
```
然后就可以执行`hexo d`发布到GitHub，访问路径`https://asiacuityz.github.io/`就可以看到页面了。
