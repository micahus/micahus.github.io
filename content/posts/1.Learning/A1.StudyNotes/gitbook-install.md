---
title: "如何编写 Gitbook"
date: 2022-03-01T01:13:27+08:00
tags: ["gitbook"]
categories: ["Learning", "StudyNotes"]
---

## 环境
nodejs
> nodejs目前的版本比较高，如果直接使用14版本，会存在gitbook安装失败，最好使用12版本
```shell
yum install nodejs
# 检查是否安装成功
node -v
```

## 安装

```shell
npm install -g gitbook-cli
# 检查是否安装成功
gitbook --version

# 出现错误 `cb.apply is not a function`，修复方法，详见`https://mizeri.github.io/2021/04/24/gitbook-cbapply-not-a-function/`

# 修改`/usr/local/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js`文件的65行起3行，具体内容如下

// fs.stat = statFix(fs.stat)
// fs.fstat = statFix(fs.fstat)
// fs.lstat = statFix(fs.lstat)
```

## 生成book
1. 如何 `html`
```shell
gitbook build ./Thief ./ThiefTarget/
```

## 配置nginx

```shell
server {
    listen 80;
    listen [::]:80;
    listen 81 http2;
    server_name gitbook.micah.wiki;
    root /xxx;
    location / {

    }
    location = /robots.txt {}
}

server {
    listen       443 ssl;
    server_name  gitbook.micah.wiki;

    ssl_certificate      /yyy/cert.pem;
    ssl_certificate_key  /yyy/key.pem;


    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    root /xxx;
    location / {

    }
    location = /robots.txt {}
}
```

## 封面
|                  |      大     |         小        |
|:----------------:|:-----------:|:-----------------:|
|     **文件**     | `cover.jpg` | `cover_small.jpg` |
| **大小（像素）** |  1800x2360  |      200x262      |

## 术语
增加 `GLOSSARY.md` 文件，貌似目前值支持英文的，不支持 GitBook 中文 术语

## 引用文件
```shell
# 1. 本地文件
{% include "./xx.md" %}
# 2. git地址
{% include "git+https://github.com/GitbookIO/documentation.git/README.md#0.0.1" %}
# 或者
git+https://[email protected]/project/blah.git/file#commit-ish
```
## 继承
编写内容类似于
```shell
{% extends "./page_info.md" %}

{% block pageContent %}
# This is my page content
{% endblock %}
```
`page_info.md` 内容
```shell
{% block pageContent %}
This is the default content
{% endblock %}

# License

{% import "./LICENSE" %}
```

## 插件
在根目录下，创建book.json，并添加以下内容
```json
{
  "plugins" : [
    "code",
    "-lunr",
    "-search",
    "search-pro"
  ]
}
```
进入根目录下，执行以下命令
```shell
gitbook install
```

## favicon
在项目根目录下新建 gitbook/images 目录
在该目录下存放 favicon 文件

通用 (尺寸 48x48) - favicon.ico
用于苹果设备 (尺寸 152x152) - apple-touch-icon-precomposed-152.png
