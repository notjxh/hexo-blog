---
title: 搭建blog
date: 2020-11-18 18:34:20
tags: hexo使用
---
内容来自[easy hexo官网](https://easyhexo.com/1-Hexo-install-and-config/)，本文只记录主要流程

---

## 1 环境准备

### 1.1 安装git

### 1.2 安装Node.js

### 1.3 安装Hexo
> npm install -g hexo-cli

## 2 本地测试
### 2.1 创建博客文件夹hexo-blog并安装hexo
> mkdir hexo-blog
hexo init hexo-blog

### 2.2 本地预览
> hexo s

### 2.3 访问
> localhost:4000

### 2.4 新建文章
> hexo new post 文章名


## 3 部署到github

### 3.1 git准备
新建一个公开仓库，仓库名格式为 your_username.github.io 例如你的 GitHub 
用户名是 easyhexo ，那么你的仓库地址名称就应该是 easyhexo.github.io

### 3.2 安装插件
> npm install hexo-deployer-git --save  
  npm install hexo-server --save

### 3.3 配置git 
_config.yml 文件中修改

```yaml
deploy:
  type: git # 类型填git
  repo: <repository url> # 你的Github仓库地址
  branch: master # 分支名称。默认填写 master 如果您使用的是 GitHub ，程序会尝试自动检测。
  message: # 提交信息可以自定义，不填的则默认为提交时间
```
### 3.4 发布到git

> $ hexo clean && hexo d -g

## 4 自定义域名  

购买域名，新增解析

## 5 源码git托管
将源代码文件托管github