

## 如果网站突然打开很慢，你会怎么处理？

​	打开Chrome调试模式(关闭缓存)，查看打开网站时的网络请求，先确定哪个或者哪些请求的响应比较慢

​	如果静态内容加载慢，怀疑网络原因；查看静态服务器带宽占用，CDN健康情况
​	如果普遍加载慢，怀疑负载原因；查看服务器带宽占用，服务器负载
​	如果PHP页面/接口普遍慢，怀疑PHP-FPM进程数或者单点资源瓶颈；检查PHP-FPM进程数、检查MySQL/Redis/MongoDB等中央服务的负载
​	如果某个或某几个页面/接口慢，怀疑其中使用的服务或外部资源问题；先看这部分代码是否有涉及共同的资源调用，比如调用外部接口，去查看相关服务是否有问题；也可能涉及一些外围问题，比如DNS解析

网站访问量变高，如何优化网站性能？
	和上一题一样，先找出瓶颈，再做优化；问题等价于，你能列举出哪些原因可能导致网站负载能力不够，并给出相应的优化措施？
	这考察的是经验，对不同网站系统在哪一块更容易发生瓶颈，怎么去处理；以下是我个人的回答

​	修复程序设计BUG：检查是否由某一处代码实现或者程序设计不合理导致网站访问变慢，加以修复，包括数据库优化
​	使用缓存：缓存是最常用的手段，并且对系统侵入性不明显，适用于重复调用并且一段时间内不变的内容；比如对MySQL查询缓存，对整个页面内容缓存(生成静态)
​	内容拆分，使用更专业的服务：静态资源使用专门服务器或者CDN；MySQL使用专门的服务器，使用SSD硬盘
​	增加服务器：简单粗暴，但是要求设计上允许横向扩展；增加PHP服务器，MySQL配置主从
​	分布式：分布式已经会对系统有明显的侵入性；简单如分表分库也算在内，使用分布式服务代替现有瓶颈的服务

​	参考：https://learnku.com/articles/24362







## 秒杀相关

- [秒杀系统架构分析与实战](https://my.oschina.net/xianggao/blog/524943)
- [商城抢购秒杀防止库存超卖方法](https://www.jianshu.com/p/48c1a92fbf3a)
- [秒杀核心设计(减库存部分)-防超卖与高并发](http://longxzq-126-com.iteye.com/blog/2228804)
- [PHP 解决并发问题的几种实现](https://blog.csdn.net/kankan231/article/details/51108450)
- [PHP 结合redis实现高并发下的抢购、秒杀功能](https://blog.csdn.net/nuli888/article/details/51865401)
- [PHP高并发下抢购、秒杀功能的超卖问题](https://my.oschina.net/programs/blog/1538228)
- [PHP 高并发秒杀解决方案（有demo）](http://www.51ask.org/article/520)
- [网站大规模并发处理方案：电商秒杀与抢购](https://www.awaimai.com/348.html)
- [PHP 秒杀系统架构设计实例](https://www.phpsong.com/2906.html)



秒杀特点

	时间短 流量高
	
	人多商品少
	
	外挂机器



技术分析

	瞬时高并发的处理能力
	多层次的分布式处理能力
	人机交互与对抗


技术选型分析
	Linux + Nginx + PHP + MySQL + Redis

		开源 免费 
	
		普及
	
	CDN，智能DNS
	
	分布式缓存、全国多节点
	
	多线路接入
	
	负载均衡LVS Nginx HAProxy
	
	大型web集群
	
	高效稳定



Nginx 开启 gzip压缩
	Tenginx 开启静态文件合并下载
	使用协商缓存

	动态程序转静态的js


数据库方面
	mysql 设计索引
	库存 unsigned 
	减少数据


分布式方案
	分布式session
	网络
	智能DNS为不同网络、不同地域的用户解析到不同的机房LVS
	



## 高并发下超卖问题？

https://my.oschina.net/programs/blog/1538228



评价多重继承的优缺点？

分布式ID



如何设计框架？

https://www.imooc.com/learn/696



## php如何解决网站大流量与高并发的问题？

预估服务器并发请求  有效访问95%



流量优化  防盗链

### 前端优化  

#### 	减少http请求  css、js、图片合并   添加异步请求  

​		1、web防盗链

​		2、减少http请求

​		3、浏览器缓存、服务器压缩传输

​		图片地图、css精灵  合并图片  使用css定位图片或者map

​		图片base64

#### 	启用浏览器缓存和文件压缩

​		nginx设置缓存策略

​		200 from cached

​		304 协商缓存    验证服务器文件是否更新  不发送响应体

​			

​		css js压缩 合并

​		gzip压缩

​		图片压缩 tinypng  jpegmini  imageoption

#### 	CDN加速

​		CDN的工作原理

​			根据用户ip判断地理位置、判断网络类型、选择路由最短和负载最轻的服务器。

​			缓存服务器没有数据，就像源站发起请求

​		bat提供cdn服务



#### 	建立独立的图片服务器  

​		独立的必要性： 分担web服务器的io负载。  可以专门优化  提高硬盘转数  cpu降低

​		采用独立域名

​		独立后的问题

​			如何进行图片上传和图片同步

​			NFS同步  FTP同步 



### 服务端优化

​	动态语言静态化  逻辑代码生成html  opcache

​	页面静态化  减少了带宽

​	动态语言的并发处理    PHP并发编程 swoole  队列

​	并发处理   队列  swoole

​	数据库优化  数据库缓存 redis 2000qps 内网流量   分库分表   分区操作  mycat tidb

​	

​    

https://coding.imooc.com/class/chapter/90.html#Anchor





如何实现 幂等



## 分布式唯一ID极简教程

<https://mp.weixin.qq.com/s/cqIK5Bv1U0mT97C7EOxmnA>



分布式session

<https://mp.weixin.qq.com/s/ucz7GhfLSD_k5Rx95PUdnw>





秒杀系统设计

https://www.imooc.com/learn/722

https://www.cnblogs.com/walblog/articles/8476579.html

优化方案1：将库存字段number字段设为unsigned，当库存为0时，因为字段不能为负数，将会返回false

2：悲观锁  每个请求需要等待锁，增大系统的平均响应时间，连接数耗尽

3：队列思路 将商品放在redis list里面

4：使用文件锁

5：使用乐观锁



引用和指针的区别



1亿个用户（服务器结构） 

3. 百万级邮件群发系统的设计 

33.对于大流量的网站,您采用什么样的方法来解决访问量问题



微博的粉丝是如何关联的

  redis list



如果用户量非常大的网站出现了性能问题时，你认为计算机那个部件会首先成为性能瓶颈？

  硬盘的读写性能

对于大流量的网站,您采用什么样的方法来解决各页面访问量统计问题



\2. 利用mysql数据库 设计一个 文章表 可以存 上亿条数据，如何设计

[http://itindex.net/detail/50227-%E6%95%B0%E6%8D%AE%E5%BA%93-%E4%BC%98%E5%8C%96-mysql](http://itindex.net/detail/50227-数据库-优化-mysql)







答：首先，确认服务器硬件是否足够支持当前的流量jmeter



其次，优化数据库访问。



第三，禁止外部的盗链。



第四，控制大文件的下载。



第五，使用不同主机分流主要流量



第六，使用流量分析统计软件







怎么设计一个高并发的秒杀系统

[http://itindex.net/detail/53237-%E7%A7%92%E6%9D%80-%E7%B3%BB%E7%BB%9F-%E8%AE%BE%E8%AE%A1](http://itindex.net/detail/53237-秒杀-系统-设计)



