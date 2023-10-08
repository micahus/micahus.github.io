---
title: "Ubuntu服务器初始化"
date: 2023-09-30T18:21:47+08:00
tags: ["ubuntu"]
categories: ["Learning", "Reflections"]
---

# 初始化需要的步骤

## 1. 更新

```shell
sudo apt upgrade
```

## 2. 安装软件

```shell
sudo apt-get install nginx
# 修改启动用户为root 
vi /etc/nginx/nginx.conf # 第一行
# user root;
```

