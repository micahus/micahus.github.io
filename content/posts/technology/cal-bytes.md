---
title: "计算容量大小"
date: 2022-07-10T10:50:10+08:00
tags: ["bytes"]
categories: ["Tech"]
---

# 计算方式

| 名称    | 计算方式                                                                                                                                                                                                                                                                                                                        |
|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| redis | http://www.redis.cn/redis_memory/                                                                                                                                                                                                                                                                                           |
| mysql | select <br/>table_schema as '数据库',<br/>sum(table_rows) as '记录数',<br/>sum(truncate(data_length/1024/1024, 2)) as '数据容量(MB)',<br/>sum(truncate(index_length/1024/1024, 2)) as '索引容量(MB)'<br/>from information_schema.tables<br/>where table_schema='mysql';<br/>参考地址： https://blog.csdn.net/fdipzone/article/details/80144166 |



