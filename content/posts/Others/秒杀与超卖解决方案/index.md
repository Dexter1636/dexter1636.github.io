+++
title = "秒杀系统设计-超卖问题解决方案"
description = ""
draft = true
math = false
type = ["posts","post"]
tags = [
    "system design",
    "development",
]
date = 2022-02-08
categories = [
    "Development",
    "System Design",
]
series = []
[ author ]
  name = "Dexter"

+++

秒杀系统在数据层面最大的问题就是超卖问题。



案例：学生抢课。

具体需求介绍，相关接口介绍。



系统设计背景：MySQL course表，sc表，student表。



**方案1：**上锁。READ FOR UPDATE，整个事务上锁。

具体逻辑：

1. 验证学号是否有效同时上锁（READ FOR UPDATE）（防止学生被删除）
2. 验证课余量同时上锁（READ FOR UPDATE）（防止被别人抢课）
3. 更新课余量
4. 写sc表

优点：保证数据的正确性。

缺点：性能低，不能承受高并发。



**方案2：**将course表和student表所有数据缓存到Redis。

具体逻辑：

1. 在Redis中检查学号是否有效
2. **是否已经选课**
3. 更新Redis中课余量-1
4. 数据持久化至MySQL（课余量大的话考虑使用MQ），如果失败则更新Redis中课余量+1



**方案2-1：**限流+缓存+MQ异步持久化。秒杀纯Redis操作，秒杀结束再写到MySQL



方案3：LRU策略进行缓存



方案4：LFU策略进行缓存
