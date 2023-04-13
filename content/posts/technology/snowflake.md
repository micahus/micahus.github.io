---
title: "雪花算法"
date: 2022-06-29T16:43:16+08:00
tags: ["java"]
categories: ["Tech"]
---

# 雪花算法

## 1. 历史

[snowflake](https://github.com/twitter-archive/snowflake)是由 twitter 开源的分布式 id 生成算法，采 用 Scala 语言实现，是把一个 64 位的 long 型的 id，1 个 bit 是不用的，用其中的 41 bits 作为毫秒数，用 10 bits 作为工作机器 id，12 bits 作为序列号。

小插曲：世界上没有两片相同的雪花，所以使用雪花来表示唯一

## 2. 算法内容

![snowflake](https://image.shijinping.cn/picgo/202206291654161.webp)

* 1:第一位不使用：为什么这里第一位不使用，因为对于long类型，如果第一位是`1` 则说明是负数

* 2～42:表示时间戳，最多可以表示2^41-1次方的数值，可以是毫秒级。
* 43～52:表示工作机器ID，最多支持2^10机器，也就是1024的机器。可以自己定义前几位为机房ID。
* 53～64:表示自增ID，同一毫秒如果超过2^12次方的增长量，应该算非常大的了

## 3. 代码实现

```java

public class SnowFlake {

    private final long workerId;
    private final long datacenterId;
    private long sequence;

    public SnowFlake(long workerId, long datacenterId, long sequence) {
        // sanity check for workerId
        // 这儿不就检查了一下，要求就是你传递进来的机房id和机器id不能超过32，不能小于0
        // 这个是二进制运算，就是 5 bit最多只能有31个数字，也就是说机器id最多只能是32以内
        // 这个是一个意思，就是 5 bit最多只能有31个数字，机房id最多只能是32以内
        long maxWorkerId = ~(-1L << workerIdBits);
        if (workerId > maxWorkerId || workerId < 0) {
            throw new IllegalArgumentException(
                    String.format("worker Id can't be greater than %d or less than 0", maxWorkerId));
        }
        long maxDatacenterId = ~(-1L << datacenterIdBits);
        if (datacenterId > maxDatacenterId || datacenterId < 0) {
            throw new IllegalArgumentException(
                    String.format("datacenter Id can't be greater than %d or less than 0", maxDatacenterId));
        }
        System.out.printf("worker starting. timestamp left shift %d, datacenter id bits %d, worker id bits %d, " +
                        "sequence bits %d, workerid %d", timestampLeftShift, datacenterIdBits, workerIdBits,
                sequenceBits, workerId);

        this.workerId = workerId;
        this.datacenterId = datacenterId;
        this.sequence = sequence;
    }

    private final long workerIdBits = 5L;
    private final long datacenterIdBits = 5L;

    private final long sequenceBits = 12L;

    private final long timestampLeftShift = sequenceBits + workerIdBits + datacenterIdBits;

    private long lastTimestamp = -1L;

    public long getWorkerId() {
        return workerId;
    }

    public long getDatacenterId() {
        return datacenterId;
    }

    public long getTimestamp() {
        return System.currentTimeMillis();
    }

    public synchronized long nextId() {
        // 这儿就是获取当前时间戳，单位是毫秒
        long timestamp = timeGen();

        if (timestamp < lastTimestamp) {
            System.err.printf("clock is moving backwards.  Rejecting requests until %d.", lastTimestamp);
            throw new RuntimeException(String.format(
                    "Clock moved backwards.  Refusing to generate id for %d milliseconds", lastTimestamp - timestamp));
        }

        if (lastTimestamp == timestamp) {
            // 这个意思是说一个毫秒内最多只能有4096个数字
            // 无论你传递多少进来，这个位运算保证始终就是在4096这个范围内，避免你自己传递个sequence超过了4096这个范围
            long sequenceMask = ~(-1L << sequenceBits);
            sequence = (sequence + 1) & sequenceMask;
            if (sequence == 0) {
                timestamp = tilNextMillis(lastTimestamp);
            }
        } else {
            sequence = 0;
        }

        // 这儿记录一下最近一次生成id的时间戳，单位是毫秒
        lastTimestamp = timestamp;

        // 这儿就是将时间戳左移，放到 41 bit那儿；
        // 将机房 id左移放到 5 bit那儿；
        // 将机器id左移放到5 bit那儿；将序号放最后12 bit；
        // 最后拼接起来成一个 64 bit的二进制数字，转换成 10 进制就是个 long 型
        long datacenterIdShift = sequenceBits + workerIdBits;
        long twepoch = 1288834974657L;
        return ((timestamp - twepoch) << timestampLeftShift) | (datacenterId << datacenterIdShift)
                | (workerId << sequenceBits) | sequence;
    }

    private long tilNextMillis(long lastTimestamp) {
        long timestamp = timeGen();
        while (timestamp <= lastTimestamp) {
            timestamp = timeGen();
        }
        return timestamp;
    }

    private long timeGen() {
        return System.currentTimeMillis();
    }

    // ---------------测试---------------
    public static void main(String[] args) {
        SnowFlake worker = new SnowFlake(1, 1, 1);
        for (int i = 0; i < 30; i++) {
            System.out.println(worker.nextId());
        }
    }

}
```



## 参考地址：

1. https://blog.csdn.net/qq_33797815/article/details/113178832