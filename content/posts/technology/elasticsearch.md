---
title: "ES学习"
date: 2022-10-05T14:30:24+08:00
tags: ["elasticsearch"]
categories: ["Tech"]
---

# ElasticSearch

> [教程](https://www.bilibili.com/video/BV1Dd4y1i7qW?vd_source=8df0e812c8a23a17177503b6ed9a3754)
>
> [文档](https://blog.csdn.net/u011863024/article/details/115721328)

## 一、安装

```shell
brew tap elastic/tap
brew install elastic/tap/elasticsearch-full
brew install elastic/tap/kibana-full
```

# 二、概念

ES于其他通用数据的对应关系

| 名称   | 通用数据库 | ES       |
| ------ | ---------- | -------- |
| 数据库 | Database   | Index    |
| 表     | Table      | Type     |
| 行     | Row        | Document |
| 列     | Column     | Fields   |
| 库名   | Schema     | Mapping  |

