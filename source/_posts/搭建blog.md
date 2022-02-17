---
title: 搭建blog
date: 2020-11-18 18:34:20
tags: hexo使用
description: 搭建blog的流程
---
内容来自[easy hexo官网](https://easyhexo.com/1-Hexo-install-and-config/)，本文只记录主要流程

---

## 环境准备

### 安装git

### 安装Node.js

### 安装Hexo
> npm install -g hexo-cli

## 本地测试
### 创建博客文件夹hexo-blog并安装hexo
> mkdir hexo-blog
hexo init hexo-blog

### 本地预览
> hexo s

### 访问
> localhost:4000

### 新建文章
> hexo new post 文章名


## 部署到github

### git准备
新建一个公开仓库，仓库名格式为 your_username.github.io 例如你的 GitHub 
用户名是 easyhexo ，那么你的仓库地址名称就应该是 easyhexo.github.io

### 安装插件
> npm install hexo-deployer-git --save  
  npm install hexo-server --save

### 配置git 
_config.yml 文件中修改

```yaml
deploy:
  type: git # 类型填git
  repo: <repository url> # 你的Github仓库地址
  branch: master # 分支名称。默认填写 master 如果您使用的是 GitHub ，程序会尝试自动检测。
  message: # 提交信息可以自定义，不填的则默认为提交时间
```
### 发布到git

> $ hexo clean && hexo d -g

## 自定义域名  

购买域名，新增解析

## 源码git托管
将源代码文件托管github

## 安装github加速插件

https://gitee.com/docmirror/dev-sidecar?_from=gitee_search

## 修改主题

访问butterfly安装文档 https://gitee.com/immyw/hexo-theme-butterfly?_from=gitee_search
在博客根目录安装稳定版
> git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly

修改hexo配置文件_config.yml，把主題改为Butterfly
> theme: butterfly

如果你沒有pug以及stylus的渲染器，請下载安裝：
> npm install hexo-renderer-pug hexo-renderer-stylus --save
