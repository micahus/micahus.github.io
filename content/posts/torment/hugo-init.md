---
title: "Hugo项目部署"
date: 2022-03-13T21:06:28+08:00 
tags: ["hugo"]
categories: ["Interest"]
---

# 安装

我喜欢安装直接下载下来，放在bin目录下，所以在 [git](https://github.com/gohugoio/hugo/releases) 的release下载对应的版本

检查安装`hugo version`查看是否已经安装成功

> 一般执行的时候，会出现告警，只要进入`系统偏好设置`->`安全性与隐私`->`通用`中`仍然允许`后，再执行一次就可以了

# 使用

1. 现在自己的目录（按照自己的习惯，我习惯`~/Workspaces/WebRoot`）下，执行`hugo new site xxx`
2. 找到自己喜欢的皮肤，个人喜欢 [even](https://github.com/olOwOlo/hugo-theme-even)
   皮肤，该皮肤从hexo-theme-even移植而来，个人感觉还不错。到新建的项目下，进入 `themes`
   目录，执行 `git clone https://github.com/olOwOlo/hugo-theme-even.git even`
3. 把`exampleSite`下的`config.toml`复制到`xxx`项目下，并根据自己的方式进行修改
4. 在 `xxx->content` 目录下，克隆你要维护的blog的markdown文档，文件夹名字命名为 `post` ，因为该theme使用的是post，而不是posts
5. 在`xxx` 目录夹运行`hugo -D`，建议先删除下`public`目录下的内容

命令如下

```shell
cd ~/Workspaces/WebRoot
hugo new site xxx
cd themes
git clone https://github.com/olOwOlo/hugo-theme-even.git even
cd ../
mv config.toml default.config.toml
cp themes/even/exampleSite/config.toml ./

# 修改自己的信息
vi config.toml

cd content

# clone 你blog的markdown地址
git clone xxx post

cd ..
rm -rf public/*
hugo -D
```

# web搭建

刚才我们安装的路径是 `~/Workspaces/WebRoot/xxx` 而hugo生成的具体内容为`~/Workspaces/WebRoot/xxx/public/` 下，所以我们需要对nginx配置地址为相应的地址

配置如下：

```shell
server {
    listen       80;
    server_name  xxx; # 这里是域名

    access_log  /Users/xxxxx/Workspaces/WebRoot/logs/xxx/access.log  main; # 这里 xxxxx 表示自己的对应目录
    error_log   /Users/xxxxx/Workspaces/WebRoot/logs/xxx/error.log;

    location / {
        root   /Users/xxxxx/Workspaces/WebRoot/xxx/public;
        index  index.html index.htm;
    }
    location /favicon.ico {
        root /Users/xxxxx/Workspaces/WebRoot;
    }
}
```

需要注意的是：nginx会获取权限失败，原因是启动的时候，需要指定用户信息`user root admin;`，并且用`sudo nginx -t`进行测试