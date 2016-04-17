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
　　** Redis 教程：**收集一些学习Redis 的教程和方法

![Redis-front](http://7xt1zm.com2.z0.glb.qiniucdn.com/static/images/blog/Redis-front.png)

===================================

### Redis 安装(Mac)
mac 上安装 redis 首先必须保证mac 已经安装 xcode.
因为make时要用到 Xcode 的command Tools (新的版本直接装tool 就可以了,不用装xcode)
(1)下载 redis   http://redis.googlecode.com/files/redis-2.8.7.tar.gz 解压到当前目录.
(2)你也可以在终端下载:
```
curl -O http://redis.googlecode.com/files/redis-2.8.7.tar.gz
sudo tar -zxf redis-2.8.7.tar.gz
```
(3)修改文件夹名,编译
```
mv redis-2.8.7 redis
cd redis/
sudo make
sudo make test
sudo make isntall
```
(4)你会在redis 目录里找到一个 redis.conf 的配置文件,打开编辑此配置文件,找到 dir  .  这一行配置.

此配置是将内存中的数据写入一个文件,这个数据库文件要保存到什么地方 就是这个配置项起到的作用.

我在mac根目录下创建了 opt 的文件夹(注意此文件夹必须有可读写权限)

所以这一行的配置是 dir  /opt/redis/

修改后保存配置文件,同时将配置文件移动到 /etc 目录下.
```
sudo mv redis.conf /etc
```
(5) 上面第三步 make install 成功后,你就应该在这个目录下看到redis
```
/usr/local/bin/redis-server
```
(6) 尝试启动一下 redis
```
/usr/local/bin/redis-server /etc/redis.conf
```
如果运行成功,你会看到下面的redis启动服务画面.


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

## Redis 常用的命令
### hashes(对象)类型的特性
          
    Redis hash 是一个 string 类型的 field 和 value 的映射表.它的添加、删除操作都是 O(1) （平均）。hash 特别适合用于存储对象。相较于将对象的每个字段存成单个 string 类型。将一个对象存
    储在 hash 类型中会占用更少的内存，并且可以更方便的存取整个对象。省内存的原因是新建一个 hash 对象时开始是用 zipmap（又称为 small hash）来存储的。这个 zipmap 其实并不
    是 hash table，但是 zipmap 相比正常的 hash 实现可以节省不少 hash 本身需要的一些元数据存储开销。尽管 zipmap 的添加，删除，查找都是 O(n)，但是由于一般对象的 field 数量都不
    太多。所以使用 zipmap 也是很快的,也就是说添加删除平均还是 O(1)。如果 field 或者 value的大小超出一定限制后，Redis 会在内部自动将 zipmap 替换成正常的 hash 实现. 这个限制可
    以在配置文件中指定
    hash-max-zipmap-entries 64 #配置字段最多 64 个
    hash-max-zipmap-value 512 #配置 value 最大为 512 字节

### lists  类型及操作

    list 是一个链表结构，主要功能是 push、pop、获取一个范围的所有值等等，操作中 key 理解为链表的名字。
    
    Redis 的 list 类型其实就是一个每个子元素都是 string 类型的双向链表。链表的最大长度是(2的 32 次方)。我们可以通过 push,pop 操作从链表的头部或者尾部添加删除元素。这使得 list
    既可以用作栈，也可以用作队列。有意思的是 list 的 pop 操作还有阻塞版本的，当我们[lr]pop 一个 list 对象时，如果 list 是空，
    或者不存在，会立即返回 nil。但是阻塞版本的 b[lr]pop 可以则可以阻塞，当然可以加超时时间，超时后也会返回 nil。为什么要阻塞版本的 pop 呢，主要是为了避免轮询。举个简单的
    例子如果我们用 list 来实现一个工作队列。执行任务的 thread 可以调用阻塞版本的 pop 去获取任务这样就可以避免轮询去检查是否有任务存在。当任务来时候工作线程可以立即返回，
    也可以避免轮询带来的延迟。
    
### sets  类型及操作

    set 的是通过 hash table 实现的，所以添加、删除和查找的复杂度都是 O(1)。hash table 会随着添加或者删除自动的调整大小。需要注意的是调整 hash table 大小时候需要同步（获取写
    锁）会阻塞其他读写操作，可能不久后就会改用跳表（skip list）来实现，跳表已经在 sortedset 中使用了。关于 set 集合类型除了基本的添加删除操作，其他有用的操作还包含集合的
    取并集(union)，交集(intersection)，差集(difference)。通过这些操作可以很容易的实现 sns中的好友推荐和 blog 的 tag 功能。   
    