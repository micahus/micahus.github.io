---
title: "JavaAtomic工具类"
date: 2023-08-25T22:21:01+08:00
tags: ["java"]
categories: ["Learning", "Reflections"]

---

# Atomic*工具类

## AtomicBoolean

### 1. 成员变量

```java
// 对应的类中的成员变量句柄
private static final VarHandle VALUE;
// 存储变量的值
private volatile int value;
```

### 2. 核心

set时使用句柄（VarHandle）来进行设置value，获取时使用value进行获取

## AtomicInteger

### 1. 成员变量

```java
private static final Unsafe U = Unsafe.getUnsafe();
private static final long VALUE = U.objectFieldOffset(AtomicInteger.class, "value");
private volatile int value;
```

### 2.核心

与Boolean类似

## AtomicLong

### 1. 成员变量

```java

```

# Atomic*Array工具类

## AtomicIntegerArray

### 1. 成员变量

```java
private static final VarHandle AA = MethodHandles.arrayElementVarHandle(int[].class);
private final int[] array;
```

