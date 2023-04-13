---
title: "阿里云盘mac本地挂在"
date: 2022-03-19T21:06:28+08:00 
tags: ["aliyundriver"]
categories: ["Interest"]
---

主要参考文档：https://blog.51cto.com/xuedingmaojun/4815572

采用方式：
https://github.com/zxbu/webdav-aliyundriver#jar包运行

挂在地址代码
```shell
cd /Users/xxx/Workspaces/WebRoot/logs/webdav;nohup java -jar /Users/xxx/Workspaces/Env/WebDAV/PATH/bin/webdav-aliyundriver-2.4.2.jar --aliyundrive.refresh-token="aaa" --server.port=aaa --aliyundrive.work-dir=/usr/local/etc/webdav/aliyundriver --aliyundrive.auth.user-name=aaa --aliyundrive.auth.password=aaa > /Users/aaa/Workspaces/WebRoot/logs/webdav/webdav.log 2>&1 &;

# nohup切换前台停止
fg

```