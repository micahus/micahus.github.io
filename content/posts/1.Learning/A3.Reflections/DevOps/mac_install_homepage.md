---
title: "Mac安装homepage"
date: 2024-03-15T21:38:06+08:00
tags: ["DevOps"]
categories: ["Learning", "Reflections"]
---

## 1. homepage介绍

官网地址：[这里](https://gethomepage.dev/latest/)

> 一个现代化、完全静态、快速、安全、完全代理、高度可定制的应用程序仪表板，集成了 100 多种服务，并可翻译成多种语言。通过 YAML 文件或 [Docker](coco://sendMessage?ext={"s%24wiki_link"%3A"https%3A%2F%2Fm.baike.com%2Fwikiid%2F1782721469276222823"}&msg=Docker) 标签发现轻松配置。

## 2. 为什么要写这个？

因为官网提供的是docker、k8s等安装方式，并没有提供mac本地安装，而我想在自己电脑上有这么一个。所以就要用源码安装了。

## 3. 普通安装会出来什么问题？

mac新版本用iTrem2的`nohup`启动后，退出iTrem2后，Session会关闭，导致`nohup`不成功

## 4. 如何处理？

### 4.1 下载源码，切换分支

```shell
git clone https://github.com/gethomepage/homepage.git HomePage
git checkout tags/v0.8.9
```

### 4.2 确定node版本，编译

```shel
# 应该在18以上，我是20
# 到工作目录
npm install
npm run build
```

### 4.3 创建plist

地址：`~/Library/LaunchAgents`

名称：`wiki.micah.homepage.plist` 注意，这里必须plist结尾的

内容：`xxx`表示你的用户目录

```shell
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>

        <key>Label</key>
        <string>wiki.micah.homepage</string>

        <key>RunAtLoad</key>
        <true/>

        <key>KeepAlive</key>
        <true/>

        <key>ThrottleInterval</key>
        <integer>60</integer>

        <key>EnvironmentVariables</key>
        <dict>
               <key>HOME_DIR</key>
               <string>/Users/xx</string>
               <key>APP_DIR</key>
               <string>/Users/xxx/HomePage</string>
        </dict>

        <key>ProgramArguments</key>
        <array>
            <string>/bin/zsh</string>
            <string>-c</string>
            <string>-l</string>
            <string>source $HOME_DIR/.zshrc; /usr/local/opt/node@20/bin/npm start</string>
        </array>
        <key>WorkingDirectory</key>
        <string>/Users/xxx/HomePage</string>

        <key>StandardOutPath</key>
        <string>/Users/xxx/HomePage/config/logs/run-info.log</string>

        <key>StandardErrorPath</key>
        <string>/Users/xxx/HomePage/config/logs/run-error.log</string>

    </dict>
</plist>
```

### 4.4 加入到启动项目中

```shell
# 加入到启动项目
launchctl load ~/Library/LaunchAgents/wiki.micah.homepage.plist
# 从启动项目删除
launchctl unload ~/Library/LaunchAgents/wiki.micah.homepage.plist
```

