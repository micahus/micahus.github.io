---
title: "Mac电脑如何配置有格式的文件夹"
date: 2023-04-20T09:01:37+08:00
tags: ["DevOps"]
categories: ["Learning", "Reflections"]
---

```shell
# 文件名命名
1. 从下面json多语言化中选择一个Key作为文件夹名
2. 在新建的文件夹名下，创建一个文件`touch .localized`

# 下面的文件地址： `/System/Library/CoreServices/SystemFolderLocalizations/zh_CN.lproj`
# 现在是二进制的，需要转换为其他格式，命令如下：
plutil -convert json SystemFolderLocalizations.strings # 该命令需要有执行权限的地方，不如拷贝到download下

# 更改图标
1. 右击查看“显示简介”
2. 命令行cd到`/System/Library/CoreServices/CoreTypes.bundle/Contents/Resources` 然后输入 `open .`
3. 找到最喜欢的图标，然后`cmd+c`，在简介中的最上面图标点击后`cmd+v`

```

```json
{
    "Saved Searches": "已存储的搜索",
    "Relocated Items": "迁移的项目",
    "Deleted Users": "已删除的用户",
    "Favorites": "个人收藏",
    "Public": "公共",
    "Compositions": "Compositions",
    "Groups": "群组",
    "Documents": "文稿",
    "Sites": "站点",
    "Network": "网络",
    "System": "系统",
    "My Network": "我的网络",
    "Shared Items": "共享的项目",
    "My Applications": "我的应用程序",
    "Movies": "影片",
    "Mail Downloads": "邮件下载",
    "Servers": "服务器",
    "Server": "服务器",
    "Pictures": "图片",
    "Shared": "共享",
    "Recovered files": "恢复的文件",
    "Configuration": "配置",
    "Utilities": "实用工具",
    "Security": "安全性",
    "Incompatible Software": "不兼容的软件",
    "Desktop": "桌面",
    "Music": "音乐",
    "Faxes": "传真",
    "Downloads": "下载",
    "Users": "用户",
    "Guest": "客人",
    "Drop Box": "投件箱",
    "Web Receipts": "网页收据",
    "Library": "资源库",
    "Local": "本地",
    "Applications": "应用程序"
}
```