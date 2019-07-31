参考：

如何保证消息中间件全链路数据100%不丢失（1）

<https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247484350&idx=1&sn=a0c52e23bea19f8b973d5deec604a693&chksm=fba6ebbdccd162ab3e8c88e34803e31c85ccc1d1218ae87345155ceff0999815a9870033d78b&scene=21#wechat_redirect>



如何保证消息中间件全链路数据100%不丢失（2）

<https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247484362&idx=1&sn=a34f070b9020d4803615fa6d7da15d93&chksm=fba6ebc9ccd162df440ad1f463ded513967beec08a92d85666a585e3810ea178e18fe0c15435&scene=21#wechat_redirect>



消息中间件如何实现消费吞吐量的百倍优化？

<https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247484372&idx=1&sn=2bfa84b2e26569d60db15a82cb684a56&chksm=fba6ebd7ccd162c13771c5345d45d40658e3815d5ed3104490a19bb4bf20fec6060a690e305b&scene=21#wechat_redirect>



高并发场景下，如何保证生产者投递到消息中间件的消息不丢失？

<https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247484377&idx=1&sn=373832e0d050bb0618448f4403f02f2a&chksm=fba6ebdaccd162cca4a26199554cbfbc37c4aec56fe54c3864a0a9cfc3255884ed212512e5cd&mpshare=1&scene=23&srcid=05274qXyJ8EmyRiqh5zx9oHz%23rd>



哥们，你们的系统架构中为什么要引入消息中间件？

<https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247484149&idx=1&sn=98186297335e13ec7222b3fd43cfae5a&chksm=fba6eaf6ccd163e0c2c3086daa725de224a97814d31e7b3f62dd3ec763b4abbb0689cc7565b0&scene=21#wechat_redirect>



消息队列的分布式事务

<https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247483876&idx=1&sn=51f8eadc16efd6b89eb1f3c6abe67e25&chksm=fba6e9e7ccd160f183b344d4773841ad8df9a2c099a16aae516ddb45aef7eaccb83570d171fe&scene=21#wechat_redirect>









如果你是基于RabbitMQ来做消息中间件的话，消费端的代码里，必须考虑三个问题：**手动ack、处理失败的nack、prefetch count的合理设置**



这三个问题背后涉及到了各种机制：



- 自动ack机制
- delivery tag机制
- ack批量与异步提交机制
- 消息重发机制
- 手动nack触发消息重发机制
- prefetch count过大导致内存溢出问题
- prefetch count过小导致吞吐量过低