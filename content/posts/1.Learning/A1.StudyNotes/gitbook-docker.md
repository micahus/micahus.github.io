---
title: "gitbook在docker内进行安装编译"
date: 2022-11-06T16:54:57+08:00
tags: ["gitbook"]
categories: ["Learning", "StudyNotes"]
---

# Gitbook与Docker

最近在学习k8s容器相关，了解了docker的优势，而本身对于特别在意环境的干净，之前的Gitbook不想安装原因，是因为要安装node等信息。借此机会尝试下使用docker进行安装。

## 1. Docker安装

这个比较简单，直接官网下载安装，无异常

## 2. docker-compose 编写

```yaml
# 在对应的目录下创建compose的yaml文件，我放在`Workspaces/Docker/GitBook`下
services:
  gitbook:
    image: bloodstar/gitbook-builder
    ports:
      - "4000:4000"
    volumes:
      - ./gitbook:/gitbook
    command: gitbook build
```

由于我只是使用gitbook的build，不需要serve，所以端口无所谓

## 3.command命令修改

### 3.1 初始化

修改command命令为`gitbook init`

### 3.2 插件安装

修改command命令为`gitbook install`，这中间会存在异常，主要是网络连接github会有一定问题

### 3.3 编译

修改command命令为`gitbook build`

## 4. 异常处理

### 4.1 初始化失败

直接建`README.md` `SUMMARY.md` 两个文件后

### 4.2 插件安装失败

需要特殊渠道，让服务可以可以访问

## 5. 部署

使用nginx做代理，直接`root`指向`Workspaces/Docker/GitBook/gitbook/_book`目录

到对应的目录夹下，运行命令`docker-compose up -d`



