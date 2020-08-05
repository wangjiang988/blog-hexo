---
title: java-volatile
date: 2020-07-27 22:05:00
tags:
    - java
    - jmm
    - volatile
    - 高并发
categories:
    - [java]
    - [jvm]
---

## 说明

volatile 关键字介绍

 <!-- more -->


### 并发编程的三大特性

1. 原子性
2. 可见性
3. 有序性
    - 指令重排概念

### JMM (java内存模型) 如何保证三大特性

1. 保证原子性
    - JMM 只保证 基本读取 赋值 的原子性操作

2. 可见性
    1. volatile
    2. synchronized
    3. JUC的lock
3. 有序性
    1. volatile
    2. synchronized
    3. JUC的lock

    - happen-before原则


### volatile深入

volatile 保证可见性 和禁止指令重排

不保证原子性的原因分析
- cpu时间片调度， 工作内存写入，主内存写入

### 使用场景

1. 并发开关
2. 状态标记利用顺序性特点（？）
3. 单例设计模式额double-check



