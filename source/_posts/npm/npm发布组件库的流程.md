---
title: npm发布组件库的流程
top: false
cover: false
toc: true
mathjax: true
date: 2021-01-26 
password: false
summary: false
tags:
    - npm
categories:
    - npm服务器
---


1、首先去npm官网注册自己的账号密码   
2、在项目文件夹中打开命令行  输入 npm login命令，然后输入用户名密码邮箱登录    
3、成功后，输入npm publish发布(此处应保证为npm原始镜像，并非淘宝镜像) 
 - 全局配置切换到淘宝源
  ```
    npm config set registry https://registry.npm.taobao.org
  ```
 - 全局配置切换到官方源
```
  npm config set registry http://www.npmjs.org
```
> 注意： 如果是淘宝镜像，则需要切换为npm原始镜像，然后重新登录。不然回报(404 错误，提示不在注册表的错误信息)

```
撤销或者删除某个包
 npm unpublish --force an_util@1.0.1
 --force 表示强制删除
```
