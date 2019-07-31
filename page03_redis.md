

## Redis基础

### 什么是Redis？

Redis 是一个开源（BSD许可）的，c语言编写的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。



### 使用Redis有哪些好处？

(1) 速度快，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1) 
(2) 支持丰富数据类型，支持string，list，set，sorted set，hash 
(3) 支持事务，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行 
(4) 丰富的特性：可用于缓存，消息，按key设置过期时间，过期后将会自动删除



### Redis有哪些数据结构？

字符串String、字典Hash、列表List、集合Set、有序集合SortedSet    

如果你是Redis中高级用户，还需要加上下面几种数据结构HyperLogLog、Geo、Pub/Sub。

如果你说还玩过Redis Module，像**BloomFilter**，RediSearch，**ReJSON**，面试官得眼睛就开始发亮了。



### Redis各个版本的区别？

2.6

3.0 cluster

3.2 地理位置

4.0 模块系统  兼容docker 

5.0 cluster 支持docker





### redis跟memcache区别

memcache多线程  redis是单线程 CPU利用方面Memcache优于Redis；  

多线程模型可以发挥多核作用，但是有些操作需要加锁，单线程的排序、聚合操作会阻塞其他命令

Memcache非阻塞IO复用的网络模型；Redis使用单线程的IO复用模型。

Memcache支持最大的存储对象大小为1M  redis最大可以达到512M

redis可以持久化 memcache 数据都在内存  宕机后无法修复数据

redis 支持多种数据结构 list set hash  memcache只有kv 

Redis支持数据的备份，即master-slave模式的数据备份。而Memcache不支持数据持久化，所以无法进行数据备份，不支持集群

它们之间底层实现方式 以及与客户端之间通信的应用协议不一样。Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。



**数据一致性保障**

Redis提供了一个“事务”的概念，虽然这是一个假的事务，由于Redis是单进程操作，所以Redis的事务仅仅只是将一组操作按顺序进行操作，在这之间不会插入任何其他命令，从而保证数据的一致性，但是这种方式很容易造成操作阻塞。
Memcached提供了类似于乐观锁一样的cas操作，会快速的返回处理成功或失败，不会对其他数据操作产生影响。
**在这一点上，Memcached的速度要比Redis更快也更安全。**



在高并发场景的压力下，多线程非阻塞式IO的Memcached表现会更加优异。



### Redis的并发竞争问题如何解决?

Redis为单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争



### Redis有哪些适合的场景？

1、会话缓存（Session Cache）

2、全页缓存（FPC）

3、队列

4、排行榜/计数器

5、发布/订阅





## 使用过Redis做异步队列么，你是怎么用的？

一般使用list结构作为队列，rpush生产消息，lpop消费消息。当lpop没有消息的时候，要适当sleep一会再重试。

list还有个指令叫blpop，在没有消息的时候，它会阻塞住直到消息到来。

使用pub/sub主题订阅者模式，可以实现1:N的消息队列。

如果对方追问pub/sub有什么缺点？

​		数据可靠性无法保证 rabbitmq 有应答机制  ack   

​		在消费者下线的情况下，生产的消息会丢失，得使用专业的消息队列如rabbitmq等。

如果对方追问redis如何实现延时队列？

​		使用sortedset，拿时间戳作为score，消息内容作为key调用zadd来生产消息，消费者用zrangebyscore指令获取N秒之前的数据轮询进行处理。

​		考虑使用专业的队列服务实现

​		



## Redis数据结构是怎么样的？

String表示的是一个可变的字节数组。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。需要注意的是字符串最大长度为512M。

List的存储结构用的是链表而不是数组，而且链表还是双向链表。因为它是链表，所以随机定位性能较弱，首尾插入删除性能较优。

Hash 结构是二维结构，第一维是数组，第二维是链表，hash的内容key和value存放在链表中，数组里存放的是链表的头指针。通过key查找元素时，先计算key的hashcode，然后用hashcode对数组的长度进行取模定位到链表的表头，再对链表进行遍历获取到相应的value值，链表的作用就是用来将产生了「hash碰撞」的元素串起来。

Set 结构内部使用hash结构，所有的value都指向同一个内部值。

Zset底层实现使用了两个数据结构，第一个是hash，第二个是跳跃列表，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。跳跃列表的目的在于给元素value排序，根据score的范围获取元素列表。





https://juejin.im/post/5b53ee7e5188251aaa2d2e16





## Redis有哪几种数据淘汰策略？

volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰
allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
no-enviction（驱逐）：禁止驱逐数据；返回错误当内存限制达到并且客户端尝试执行会让更多内存被使用的命令



https://www.cnblogs.com/WJ5888/p/4371647.html

​	立即删除  需要消耗cpu
​	惰性删除  访问的时候在删除 
​	定时删除  可以在cpu空闲的时候删除



## Redis回收进程如何工作的？

一个客户端运行了新的命令，添加了新的数据。

Redi检查内存使用情况，如果大于maxmemory的限制, 则根据设定好的策略进行回收。

一个新的命令被执行，等等。

所以我们不断地穿越内存限制的边界，通过不断达到边界然后不断地回收回到边界以下。

如果一个命令的结果导致大量内存被使用（例如很大的集合的交集保存到一个新的键），不用多久内存限制就会被这个内存使用量超越。



### 谈谈redis事务？

Redis Cluster集群架构，不同的key是有可能分配在不同的Redis节点上的，在这种情况下Redis的事务机制是不生效的。其次，Redis事务不支持回滚操作，简直是鸡肋！所以基本不用！





### Redis如何批量操作？

单机redis可以使用mset、mget操作等
Redis Cluster集群架构，不同的key会划分到不同的slot中，因此直接使用mset或者mget等操作是行不通的。



### 那在Redis集群模式下，如何进行批量操作？

这里说一种有一个很简单的答法，足够面试用。即:
如果执行的key数量比较少，就不用mget了，就用串行get操作。如果真的需要执行的key很多，就使用Hashtag保证这些key映射到同一台Redis节点上。简单来说语法如下

对于key为{foo}.student1、{foo}.student2，{foo}student3，这类key一定是在同一个redis节点上。因为key中“{}”之间的字符串就是当前key的hash tags， 只有key中{ }中的部分才被用来做hash，因此计算出来的redis节点一定是同一个!

ps:如果你用的是Proxy分片集群架构，例如Codis这种，会将mget/mset的多个key拆分成多个命令发往不同得Redis实例，这里不多说。我推荐答的还是Redis Cluster。





### redis如何做持久化

Redis提供了两种不同的持久化模式：

RDB 快照模式，bgsave做镜像全量持久化,该模式用于生成某个时间点的备份信息，并且会对当前的key value进行编码存储到rdb文件中

AOF 增量持久化模式，该模式类似binlog的形式，会记录服务器所有的写请求，在服务重启的时候通过回放执行命令请求来恢复原有的数据。如果不要求性能，在每条写指令时都sync一下磁盘，就不会丢失数据。定时sync，比如1s1次，这个时候最多就会丢失1s的数据。



bgsave的原理是什么？

bgsave：该触发方式会fork一个子进程，由子进程负责持久化过程，因此阻塞只会发生在fork子进程的时候。

你给出两个词汇就可以了，fork和cow。fork是指redis通过创建子进程来进行bgsave操作，cow指的是copy on write，子进程创建后，父子进程共享数据段，父进程继续提供读写服务，写脏的页面数据会逐渐和子进程分离开来。



### Pipeline有什么好处，为什么要用pipeline？

可以将多次IO往返的时间缩减为一次，前提是pipeline执行的指令之间没有因果相关性。使用redis-benchmark进行压测的时候可以发现影响redis的QPS峰值的一个重要因素是pipeline批次指令的数目。



## Redis主从复制

### Redis主从复制的原理？

1. 如果设置了一个Slave，无论是第一次连接还是重连到Master，它都会发出一个SYNC命令；
2. 当Master收到SYNC命令之后，会做两件事：
a) Master执行BGSAVE，即在后台保存数据到磁盘（rdb快照文件）；
b) Master同时将新收到的写入和修改数据集的命令存入缓冲区（非查询类）；
3. 当Master在后台把数据保存到快照文件完成之后，Master会把这个快照文件传送给Slave，而Slave则把内存清空后，加载该文件到内存中；
4. 而Master也会把此前收集到缓冲区中的命令，通过Reids命令协议形式转发给Slave，Slave执行这些命令，实现和Master的同步；
5. Master/Slave此后会不断通过异步方式进行命令的同步，达到最终数据的同步一致；
6. 需要注意的是Master和Slave之间一旦发生重连都会引发全量同步操作。但在2.8之后版本，也可能是部分同步操作。

部分复制
2.8开始，当Master和Slave之间的连接断开之后，他们之间可以采用持续复制处理方式代替采用全量同步。
Master端为复制流维护一个内存缓冲区（in-memory backlog），记录最近发送的复制流命令；同时，Master和Slave之间都维护一个复制偏移量(replication offset)和当前Master服务器ID（Master run id）。当网络断开，Slave尝试重连时：
a. 如果MasterID相同（即仍是断网前的Master服务器），并且从断开时到当前时刻的历史命令依然在Master的内存缓冲区中存在，则Master会将缺失的这段时间的所有命令发送给Slave执行，然后复制工作就可以继续执行了；
b. 否则，依然需要全量复制操作；

Redis 2.8 的这个部分重同步特性会用到一个新增的 PSYNC 内部命令， 而 Redis 2.8 以前的旧版本只有 SYNC 命令， 不过， 只要从服务器是 Redis 2.8 或以上的版本， 它就会根据主服务器的版本来决定到底是使用 PSYNC 还是 SYNC ：

如果主服务器是 Redis 2.8 或以上版本，那么从服务器使用 PSYNC 命令来进行同步。
如果主服务器是 Redis 2.8 之前的版本，那么从服务器使用 SYNC 命令来进行同步.



## 解决单点问题主要有2种方式：

### 主备方式 

这种通常是一台主机、一台或多台备机，在正常情况下主机对外提供服务，并把数据同步到备机，当主机宕机后，备机立刻开始服务。 
Redis HA中使用比较多的是keepalived，它使主机备机对外提供同一个虚拟IP，客户端通过虚拟IP进行数据操作，正常期间主机一直对外提供服务，宕机后VIP自动漂移到备机上。

优点是对客户端毫无影响，仍然通过VIP操作。 
缺点也很明显，在绝大多数时间内备机是一直没使用，被浪费着的。



### 主从方式 

这种采取一主多从的办法，主从之间进行数据同步。 当Master宕机后，通过选举算法(Paxos、Raft)从slave中选举出新Master继续对外提供服务，主机恢复后以slave的身份重新加入。 
主从另一个目的是进行读写分离，这是当单机读写压力过高的一种通用型解决方案。 其主机的角色只提供写操作或少量的读，把多余读请求通过负载均衡算法分流到单个或多个slave服务器上。

缺点是主机宕机后，Slave虽然被选举成新Master了，但对外提供的IP服务地址却发生变化了，意味着会影响到客户端。 解决这种情况需要一些额外的工作，在当主机地址发生变化后及时通知到客户端，客户端收到新地址后，使用新地址继续发送新请求。



### Redis的多数据库机制，了解多少？

Redis单机支持多个数据库，并且每个数据库的数据是隔离的不能共享，单机下的redis可以支持16个数据库（db0 ~ db15）

redis 的多库扩容难

在Redis Cluster集群架构下只有一个数据库空间，即db0。因此，我们没有使用Redis的多数据库功能！



### Redis集群机制中，你觉得有什么不足的地方吗？

假设我有一个key，对应的value是Hash类型的。如果Hash对象非常大，是不支持映射到不同节点的！只能映射到集群中的一个节点上！还有就是做批量操作比较麻烦！

我们知道Redis支持“multiple key”操作（即多个key的数据在一次调用中返回），那么Cluster也支持此特性，但是要求所有的keys需要在一个hash slot上（即在同一个node上），为了确保多个keys所属与同一个slot，则要求使用“hash tags”。

hash tags”：Redis Cluster支持的特性，在key字符串中如果包含"{}"，那么“{}”之间的字符串即为“hash tags”，客户端（包括Server端）将根据“hash tags”来计算此key所属的slots，而不是key完整的字符串，当然存储仍使用key。所以，有了“hash tags”，我们可以强制某些keys被保存在相同的slots：比如，“foo”和“{foo}.another”将会保存同一个slot上。这对“multiple key”操作非常有帮助。





### 你们有对Redis做读写分离么？

不做读写分离。我们用的是Redis Cluster的架构，是属于分片集群的架构。而Redis本身在内存上操作，不会涉及IO吞吐，即使读写分离也不会提升太多性能，Redis在生产上的主要问题是考虑容量，单机最多10-20G，key太多降低Redis性能.因此采用分片集群结构，已经能保证了我们的性能。其次，用上了读写分离后，还要考虑主从一致性，主从延迟等问题，徒增业务复杂度。



### Redis集群方案应该怎么做？都有哪些方案？

1.twemproxy，大概概念是，它类似于一个代理方式，使用方法和普通redis无任何区别，设置好它下属的多个redis实例后，使用时在本需要连接redis的地方改为连接twemproxy，它会以一个代理的身份接收请求并使用一致性hash算法，将请求转接到具体redis，将结果再返回twemproxy。使用方式简便(相对redis只需修改连接端口)，对旧项目扩展的首选。 问题：twemproxy自身单端口实例的压力，使用一致性hash后，对redis节点数量改变时候的计算值的改变，数据无法自动移动到新的节点。

2.codis，目前用的最多的集群方案，基本和twemproxy一致的效果，但它支持在 节点数量改变情况下，旧节点数据可恢复到新hash节点。

3.redis cluster3.0自带的集群，特点在于他的分布式算法不是一致性hash，而是hash槽的概念，以及自身支持节点设置从节点。具体看官方文档介绍。

4.在业务代码层实现，起几个毫无关联的redis实例，在代码层，对key 进行hash计算，然后去对应的redis实例操作数据。 这种方式对hash层代码要求比较高，考虑部分包括，节点失效后的替代算法方案，数据震荡后的自动脚本恢复，实例的监控，等等。



#### Replication+Sentinel

![](images\redis_sentinel.png)

Sentinel的作用有三个:
监控:Sentinel 会不断的检查主服务器和从服务器是否正常运行。

通知:当被监控的某个Redis服务器出现问题，Sentinel通过API脚本向管理员或者其他的应用程序发送通知。

自动故障转移:当主节点不能正常工作时，Sentinel会开始一次自动的故障转移操作，它会将与失效主节点是主从关系的其中一个从节点升级为新的主节点，并且将其他的从节点指向新的主节点。

工作原理就是，当Master宕机的时候，Sentinel会选举出新的Master，并根据Sentinel中client-reconfig-script脚本配置的内容，去动态修改VIP(虚拟IP)，将VIP(虚拟IP)指向新的Master。我们的客户端就连向指定的VIP即可！
故障发生后的转移情况，可以理解为下图



![](images\redis_sentinel2.png)



缺陷:
(1)主从切换的过程中会丢数据
(2)Redis只能单点写，不能水平扩容



#### Proxy+Replication+Sentinel

Codis和Twemproxy

![](images\redis_twemproxy_sentinel.png)



缺陷:
(1)部署结构超级复杂
(2)可扩展性差，进行扩缩容需要手动干预
(3)运维不方便



### redis cluster有没有了解过，怎么做到高可用的？



#### Redis Cluster

![](images\redis_cluster.png)



工作原理如下

客户端与Redis节点直连,不需要中间Proxy层，直接连接任意一个Master节点

根据公式HASH_SLOT=CRC16(key) mod 16384，计算出映射到哪个分片上，然后Redis会去相应的节点进行操作

具有如下优点:
(1)无需Sentinel哨兵监控，如果Master挂了，Redis Cluster内部自动将Slave切换Master
(2)可以进行水平扩容
(3)支持自动化迁移，当出现某个Slave宕机了，那么就只有Master了，这时候的高可用性就无法很好的保证了，万一Master也宕机了，咋办呢？ 针对这种情况，如果说其他Master有多余的Slave ，集群自动把多余的Slave迁移到没有Slave的Master 中。



Redis 集群是一个提供在多个Redis间节点间共享数据的程序集。

为了使在部分节点失败或者大部分节点无法通信的情况下集群仍然可用，所以集群使用了主从复制模型,每个节点都会有N-1个复制品.

在集群创建的时候（或者过一段时间）我们为每个节点添加一个从节点A1，B1，C1,那么整个集群便有三个master节点和三个slave节点组成，这样在节点B失败后，集群便会选举B1为新的主节点继续服务，整个集群便不会因为槽找不到而不可用了

不过当B和B1 都失败后，集群是不可用的.

Redis 并不能保证数据的强一致性

要让集群正常运作至少需要三个主节点，不过在刚开始试用集群功能时， 强烈建议使用六个节点： 其中三个为主节点， 而其余三个则是各个主节点的从节点。



Redis 集群通过分区来提供一定程度的可用性,在实际环境中当某个节点宕机或者不可达的情况下继续处理命令. Redis 集群的优势:

自动分割数据到不同的节点上。
整个集群的部分节点失败或者不可达的情况下能够继续处理命令。



缺点:

不支持keys

(1)批量操作是个坑
(2)资源隔离性较差，容易出现相互影响的情况。



**是否使用过Redis集群，集群的原理是什么？**

Redis Sentinal着眼于高可用，在master宕机时会自动将slave提升为master，继续提供服务。

Redis Cluster着眼于扩展性，在单个redis内存不足时，使用Cluster进行分片存储。





### Redis持久化数据和缓存怎么做扩容？

如果Redis被当做缓存使用，使用一致性哈希实现动态扩容缩容。

如果Redis被当做一个持久化存储使用，必须使用固定的keys-to-nodes映射关系，节点的数量一旦确定不能变化。否则的话(即Redis节点需要动态变化的情况），必须使用可以在运行时进行数据再平衡的一套系统，而当前只有Redis集群可以做到这样。



### 如果有大量的key需要设置同一时间过期，一般需要注意什么？

如果大量的key过期时间设置的过于集中，到过期的那个时间点，redis可能会出现短暂的卡顿现象。一般需要在时间上加一个随机值，使得过期时间分散一些。



### 假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？

使用keys指令可以扫出指定模式的key列表。

对方接着追问：如果这个redis正在给线上的业务提供服务，那使用keys指令会有什么问题？

这个时候你要回答redis关键的一个特性：redis的单线程的。keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。这个时候可以使用scan指令，scan指令可以无阻塞的提取出指定模式的key列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用keys指令长。





### Memcache的工作原理是什么？

Memcache的工作就是在专门的机器内存里维护一张巨大的hash表，存储经常被读写的一些文件与数据，从而极大地提高网站的运行效率。



### redis如何实现高并发分布式锁



http://redis.cn/topics/distlock.html



```
SET resource_name my_random_value NX PX 30000		
```



先拿setnx来争抢锁，抢到之后，再用expire给锁加一个过期时间防止锁忘记了释放。

这时候对方会告诉你说你回答得不错，然后接着问如果在setnx之后执行expire之前进程意外crash或者要重启维护了，那会怎么样？

这时候你要给予惊讶的反馈：唉，是喔，这个锁就永远得不到释放了。紧接着你需要抓一抓自己得脑袋，故作思考片刻，好像接下来的结果是你主动思考出来的，然后回答：我记得set指令有非常复杂的参数，这个应该是可以同时把setnx和expire合成一条指令来用的！



https://juejin.im/entry/5a502ac2518825732b19a595



### redis为什么是原子操作？

### 为何单线程的 Redis 却能支撑高并发？



​	参考：https://blog.csdn.net/zl1zl2zl3/article/details/89791758



### redis如何利用多核CPU

一般服务器的cpu都不会是性能瓶颈。redis的性能瓶颈主要集中在内存和网络方面。
如果你确实需要充分使用多核cpu的能力，那么需要在单台服务器上运行多个redis实例(主从部署/集群化部署)，并将每个redis实例和cpu内核进行绑定





### 说说Redis哈希槽的概念？

Redis集群没有使用一致性hash,而是引入了哈希槽的概念，Redis集群有16384个哈希槽，每个key通过CRC16校验后对16384取模来决定放置哪个槽，集群的每个节点负责一部分hash槽。







https://www.cnblogs.com/DarrenChan/p/8796922.html#_label1_18



Redis集群的主从复制模型是怎样的？

20、Redis集群会有写操作丢失吗？为什么？

21、Redis集群之间是如何复制的？

22、Redis集群最大节点个数是多少？

23、Redis集群如何选择数据库？

24、怎么测试Redis的连通性？

25、Redis中的管道有什么用？

26、怎么理解Redis事务？

27、Redis事务相关的命令有哪几个？

28、Redis key的过期时间和永久有效分别怎么设置？

29、Redis如何做内存优化？

30、Redis回收进程如何工作的？

31、Redis回收使用的是什么算法？

32、Redis如何做大量数据插入？

33、为什么要做Redis分区？

34、你知道有哪些Redis分区实现方案？

35、Redis分区有什么缺点？

36、Redis持久化数据和缓存怎么做扩容？

37、分布式Redis是前期做还是后期规模上来了再做好？为什么？

38、Twemproxy是什么？

39、支持一致性哈希的客户端有哪些？

40、Redis与其他key-value存储有什么不同？

41、Redis的内存占用情况怎么样？

42、都有哪些办法可以降低Redis的内存使用情况呢？

43、查看Redis使用情况及状态信息用什么命令？

44、Redis的内存用完了会发生什么？

45、Redis是单线程的，如何提高多核CPU的利用率？

46、一个Redis实例最多能存放多少的keys？List、Set、Sorted Set他们最多能存放多少元素？

47、Redis常见性能问题和解决方案？

48、Redis提供了哪几种持久化方式？

49、如何选择合适的持久化方式？

50、修改配置不重启Redis会实时生效吗？

### redis 内存是如何分配的?Redis 内存分配分析





### 如何保证redis和mysql数据一致？

​	缓存更新策略









### redis常见性能问题和解决方案：

(1) Master最好不要做任何持久化工作，如RDB内存快照和AOF日志文件 
(2) 如果数据比较重要，某个Slave开启AOF备份数据，策略设置为每秒同步一次 
(3) 为了主从复制的速度和连接的稳定性，Master和Slave最好在同一个局域网内 
(4) 尽量避免在压力很大的主库上增加从库 
(5) 主从复制不要用图状结构，用单向链表结构更为稳定，即：Master <- Slave1 <- Slave2 <- Slave3… 
这样的结构方便解决单点故障问题，实现Slave对Master的替换。如果Master挂了，可以立刻启用Slave1做Master，其他不变。



进阶

[神奇的HyperLogLog算法](http://www.rainybowe.com/blog/2017/07/13/神奇的HyperLogLog算法/index.html)

[Redis如何执行命令](https://zhuanlan.zhihu.com/p/54953927)



Redis
	参考：
https://segmentfault.com/a/1190000018237937
[Redis面试题及分布式集群](https://blog.csdn.net/yajlv/article/details/73467865)
[Redis面试题总结](https://blog.csdn.net/ydyang1126/article/details/72667602)
[天下无难试之Redis面试题刁难大全](https://zhuanlan.zhihu.com/p/32540678)
[史上最全Redis面试题及答案](https://www.jianshu.com/p/85d55f2ffd0a)
[Redis 与 Memcache](https://blog.csdn.net/hguisu/article/details/8836819)
[Redis 数据类型及应用场景](https://segmentfault.com/a/1190000012212663)
[Redis中的五种数据类型使用场景](https://www.jianshu.com/p/d645cebff386)
[Redis 和 Memcached 各有什么优缺点，主要的应用场景是什么样的](https://www.zhihu.com/question/19829601)
[Redis与Memcached的区别](http://blog.51cto.com/gnucto/998509)
[面试中关于Redis的问题看这篇就够了](https://juejin.im/post/5ad6e4066fb9a028d82c4b66)
[一文轻松搞懂redis集群原理及搭建与使用](https://juejin.im/post/5ad54d76f265da23970759d3)



