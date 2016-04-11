title:  Redis 教程
date: 2016-04-11 16:58:00
description: Redis 学习，教程
categories:
- Redis
tags:
- java
- 教程
toc: true
author: Louis Wang
comments:
original:
permalink: 
---
　　** Redis 教程：**收集一些学习Redis 的教程和方法。
===================================

## Key-Value Store 的特性
          
    Key-Value Store 最大的特点就是它的可扩展性，这也就是它最大的优势。所谓的可扩展性，在我看来这里包括了两方面内容。一方面，是指 Key-Value Store 可以支持极大的数据的存储，
    它的分布式的架构决定了只要有更多的机器，就能够保证存储更多的数据。另一方面，是指它可以支持数量很多的并发的查询。
    对于 RDBMS，一般几百个并发的查询就可以让它很吃力了，而一个 Key-Value Store，可以很轻松的支持上千的并发查询。下面而简单的罗列了一些特  
      
> - Key-value store：一个 key-value 数据存储系统，只支持一些基本操作，如：SET(key, value)和 GET(key) 等；
> - 分布式：多台机器（nodes）同时存储数据和状态，彼此交换消息来保持数据一致，可视为一个完整的存储系统；
> - 数据一致：所有机器上的数据都是同步更新的、不用担心得到不一致的结果；
> - 冗余：所有机器（nodes）保存相同的数据，整个系统的存储能力取决于单台机器（node）的能力；
> - 容错：如果有少数 nodes 出错，比如重启、当机、断网、网络丢包等各种 fault/fail 都不影响整个系统的运行；
> - 高可靠性：容错、冗余等保证了数据库系统的可靠性。

