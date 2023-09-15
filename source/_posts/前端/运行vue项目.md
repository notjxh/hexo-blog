---
title: 运行vue项目
date: 2023-09-14 14:47:42
tags: vue
description: 
categories: 前端

---

# 运行vue项目

## 下载并安装node.js

> node -v 

## 安装webpack

> npm install webpack -g

## 安装vue-cli脚手架

> npm install vue-cli -g

安装vue3需要用以下的命令

> npm uninstall -g vue-cli
> npm install -g @vue/cli

查看是否安装成功

> vue -V

## 使用vue-cli创建项目

> vue create hello-world

hello-world是vue项目名

## 运行项目

> cd hello-world

> npm run serve

localhost:8080出现页面表示成功

# 运行别人的项目

## 删除package-lock.json文件

## 进入项目目录清除缓存

> npm cache clean -f

## 重新安装依赖然后运行

> npm install
> npm run serve

# 管理多版本nodejs

## 卸载node

> where node

## 安装nvm

> nvm -v 查看版本

## 修改镜像

打开 nvm的安装目录，修改  settings.txt  文件添加

> node_mirror: https://npm.taobao.org/mirrors/node/
> npm_mirror: https://npm.taobao.org/mirrors/npm/

## 查看可用版本的nodejs并安装

> nvm ls available 查看可用
> nvm install 12.20.0 安装
> nvm use 12.20.0 使用

## 查看安装并卸载

> nvm ls
> nvm uninstall 12.20.0

## nvm常用命令

> nvm list 是查找本电脑上所有的node版本
> nvm list 查看已经安装的版本
> nvm list installed 查看已经安装的版本
> nvm list available 查看网络可以安装的版本
> nvm install 安装最新版本nvm
> nvm use <version> ## 切换使用指定的版本node
> nvm ls 列出所有版本
> nvm current显示当前版本
> nvm alias <name> <version> ## 给不同的版本号添加别名
> nvm unalias <name> ## 删除已定义的别名
> nvm reinstall-packages <version> ## 在当前版本node环境下，重新全局安装指定版本号的npm包
> nvm on 打开nodejs控制
> nvm off 关闭nodejs控制
> nvm proxy 查看设置与代理
> nvm node_mirror [url] 设置或者查看setting.txt中的node_mirror，如果不设置的默认是 https://nodejs.org/dist/
> nvm npm_mirror [url] 设置或者查看setting.txt中的npm_mirror,如果不设置的话默认的是： https://github.com/npm/npm/archive/.
> nvm uninstall <version> 卸载制定的版本
> nvm use [version] [arch] 切换制定的node版本和位数
> nvm root [path] 设置和查看root路径
> nvm version 查看当前的版本
