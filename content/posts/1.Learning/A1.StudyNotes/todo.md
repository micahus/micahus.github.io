---
title: "TodoList"
date: 2023-04-13T19:57:12+08:00
tags: ["待完成"]
categories: ["Learning", "StudyNotes"]
---

TODO未整理

golang的gc，gmp模型，context作用，channel原理，并发打印数字，slice和map原理
kafka的重平衡，高水位，顺序消费，怎么保证消息不丢失
rocketmq怎么实现事物消息
redis数据结构，zset原理，介绍cluster已经怎么保证高可用，哨兵模式介绍
mysql聚簇索引，索引优化，结合业务怎么分库分表，为啥一个表超过1000w性能会变差

压测，限流
降怎么保证服务高可用，限流熔断降级压测都要提下
监控报警这些
限流有哪些算法，以及却别要知道下
熔断策略是啥
限流：窗口计数，滑动窗口，漏桶，令牌桶





1. webrtc是什么技术
2. webrtc与不同的socket通信有什么区别
3. webrtc如何实现连麦、直播的



![image-20230322152028665](https://image.shijinping.cn/picgo/20230322152028665.png)

# 计算方式

| 名称  | 计算方式                                                     |
| ----- | ------------------------------------------------------------ |
| redis | http://www.redis.cn/redis_memory/                            |
| mysql | select <br/>table_schema as '数据库',<br/>sum(table_rows) as '记录数',<br/>sum(truncate(data_length/1024/1024, 2)) as '数据容量(MB)',<br/>sum(truncate(index_length/1024/1024, 2)) as '索引容量(MB)'<br/>from information_schema.tables<br/>where table_schema='mysql';<br/>参考地址： https://blog.csdn.net/fdipzone/article/details/80144166 |



