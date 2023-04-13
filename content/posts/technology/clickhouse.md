---
title: "ClickHouse"
date: 2022-10-02T14:54:45+08:00
tags: ["clickhouse"]
categories: ["Tech"]
---

# ClickHouse

> 官网：[clickhouse.com](https://clickhouse.com/)
>
> 学习资料：[谷粒](https://www.gulixueyuan.com/my/course/445)

## 核心要点：

1. MergeTree引擎
2. OrderBy是主键
3. 分布式
4. Explain
5. 参数配置
6. 语法规则
7. 多表联查（join）



## 面试题

1.不支持真正的`delete/update`操作，不支持`transactions`(事物)

```text
OLAP引擎一般都不支持事物，ClickHouse的定位也是分析性数据库，而不是严格的关系型数据库，加入对于事物的支持，
必然会有锁，同时分布式事物的支持，会带来更复杂的实现，其中诸多因素，都会影响写入和查询的性能。
```

2.不支持高并发查询，官方建议`100 QPS`

```text
ClickHouse是并行计算，单个查询就可以跑满多个CPU核心，而不像MySQL单个查询单线程执行。
```

3.需要批量写入，频繁的单条写入会带来写入问题

```text
ClickHouse存储结构有点类LSM，每次的insert基本都会生成一个文件目录，后台线程Merge目录文件，如果频繁写入，
后台线程就会Merge不过来，产生`Too many parts`异常。建议每秒不超过一次写入，并且是Batch写入。
```

4.有限的SQL语法支持，JOIN语法也比较另类，暂时不支持窗口函数

5.稀疏索引的设计使得ClickHouse不适合做单行点查询