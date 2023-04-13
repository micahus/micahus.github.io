---
title: "Java线程"
date: 2022-06-02T10:22:45+08:00
tags: ["java"]
categories: ["Tech"]
---

# 线程

## 概念
1. Java线程是什么  
> Java的线程，是运行在JVM的程序上的基本执行单元， Java针对线程抽象出Thread对象的概念。

2. Java线程分类
> Thread分为守护线程和非守护线程，当JVM启动时，伴随一个非守护线程的运行，我们称之为主线程/main函数，当JVM所有的非守护线程都销毁时，JVM实例也会销毁；

3. Java线程生命周期
> Java线程包含6个状态：NEW，RUNNABLE，BLOCKED，WAITING，TIMED_WAITING，TERMINATED

4. 多线程优缺点
> JVM支持多线程，正确使用多线程能大大提高程序的服务能力，同时也引入程序的复杂度和线程安全问题(不正确使用)。

## 状态`Thread.State`
> 一个线程在指定的时刻上，只能存在一个状态；JVM的线程状态和操作系统的线程状态不是一一对应的。了解线程状态可用于分析线程问题/监控，不建议通过判断线程状态来进行逻辑处理

![Thread-20b08934](https://image.shijinping.cn/picgo/202206021024473.png)

* `NEW` (新建)       
> 一个尚未启动的线程处于这一状态。(A thread that has not yet started is in this state.)

尚未启动的线程处于这一状态，即尚未调用start()
```Java
Theard t = new Theard();
```

* `RUNNABLE` (可运行)       
> 一个正在 Java 虚拟机中执行的线程处于这一状态。(A thread executing in the Java virtual machine is in this state.)

JVM中可执行的线程处于这一状态
```Java
Theard t = new Theard();
t.start();
```

* `BLOCKED` (阻塞)       
> 一个正在阻塞等待一个监视器锁的线程处于这一状态。(A thread that is blocked waiting for a monitor lock is in this state.)

线程等待监视器锁时处于这一状态，如 synchronized，通俗理解：当因为获取不到锁而无法进入同步块时，线程处于 BLOCKED 状态。

* `WAITING` (等待)       
> 一个正在无限期等待另一个线程执行一个特别的动作的线程处于这一状态。(A thread that is waiting indefinitely for another thread to perform a particular action is in this state.)

正在无限期等待另一个线程执行一个特别的动作的线程处于这一状态，如 `Object.wait()`、`Thread.join()`、`LockSupport.park()`。特别动作分别为：`notify/notifyAll`、线程完结、、`LockSupport.unpark()`

* `TIMED_WAITING` (计时等待)       
> 一个正在限时等待另一个线程执行一个动作的线程处于这一状态。(A thread that is waiting for another thread to perform an action for up to a specified waiting time is in this state.)

限时等待另一个线程执行一个动作的线程处于这一状态，如：`Thread.sleep(long millis)`、`Object.wait(long millis`，`Object.wait(long millis)`、`Thread.join(long millis)`、`LockSupport.parkNanos(Object blocker, long nanos) `、 `LockSupport.parkUntil(Object blocker, long nanos)`

* `TERMINATED` (终止)       
> 一个已经退出的线程处于这一状态。(A thread that has exited is in this state.)

已经退出的线程处于这一状态

## 方法
* 常用新建线程

```Java
// 1. 默认新建
Thread t1 = new Thread();

// 2. 传入runnable方法
Thread t2 = new Thread(new Runnable() {
	public void run() {
		synchronized (xxx) {
			// todo
		}
	}
});

// 3. 传入默认名称
Thread t3 = new Thread("thread-name");

// 还有些是其他方法，详细Thread类

```

* 线程开始

```Java
/**
 * Causes this thread to begin execution; the Java Virtual Machine calls the run method of this thread.
 */
start();
```
该方法代码实现是调用 `start0();` 方法，但实际上是调用Thread中的 `run()` 方法；

* 退出（内部方法）

```Java
/**
 * This method is called by the system to give a Thread
 * a chance to clean up before it actually exits.
 */
exit();
```

```Java
/**
 * 只是在当前线程中打了一个停止的标记，并不是直接将线程停止
 */
interrupt();
```




## 其他
线程优先级别


## 资料索引
1. [国栋的osChina](https://my.oschina.net/goldenshaw?tab=newest&catalogId=3277710)