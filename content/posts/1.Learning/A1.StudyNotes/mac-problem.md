---
title: "苹果系统问题"
date: 2022-03-01T01:04:18+08:00
tags: ["mac"]
categories: ["Learning", "StudyNotes"]
---

## 1.邮箱无法删除
菜单选择：邮箱 -> 重建

## 2. Your have Mail
找到`/var/mail`，如果里面有内容。
使用 `mail` 查看具体内容或使用 `rm /var/mail/$USER` 进行删除

## 3. locale问题 远程登录时 `LC_CTYPE: cannot change locale (UTF-8)`
在.zshrc中添加以下命令
```
export LC_ALL=en_US.UTF-8  
export LANG=en_US.UTF-8
```

## 4. image2icon
mac上的图标不是新版本，看上去很不顺眼，可以使用该软件，生成icns后，然后使用“查看简介”，直接拖动到左上角的图标上 `xclient`
