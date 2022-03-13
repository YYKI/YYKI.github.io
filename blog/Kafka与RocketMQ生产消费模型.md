# Kafka与RocketMQ生产消费模型
## 架构图
### Kafka
基本概念：producer, consumer, consumerGroup, topic, message, broker, partition(leader, follower), zookeeper(kafka3不再依赖zookeeper)
![图片](https://user-images.githubusercontent.com/24954102/158013057-7d0ceaa9-708b-46f3-a248-0ada0a527de6.png)

### RocketMQ
基本概念：producer, consumer, consumerGroup, topic, message, broker(leader, follower), queue, commitLog, nameServer
![图片](https://user-images.githubusercontent.com/24954102/158013831-310acc92-fde5-4a80-aca8-1fa83551c6c3.png)


## 生产
### Kafka
分区（partition）选择
- 轮询策略
- 随机策略
- key-ordering策略(xiao)

消息合并批量压缩后发送(节省网络传输资源占用)

### RocketMQ
队列（queue）选择
- 轮询
- 自定义：MessageQueueSelector

消息单条压缩发送(批量消息存在gc问题)

支持延时消息

## 消费
### consumer
每个分区或队列只能被一个消费者消费（同一个消费组），消费组内的消费者数目大于分区数量时，部分消费者会处于空闲状态

有consumer加入或退出时会做rebalance操作，重新分配

### offset
每个分区的消费进度offset，本地和远程都会保存一份；offset(偏移量)可以自动或手动提交，默认自动

### 多线程消费
Kafka Consumer#poll 业务自己实现多线程：（1）多个线程，多个消费者 （2）一个消费者，多业务线程处理拉取后的数据，不支持多线程同一个消费者并发操作

RocketMQ Consumer+MessageLisenter 可配置

### 消息堆积，流量控制
消费端消息获取有两种模式，推和拉；

Kafka采用拉模式（短轮询）；

RocketMQ采用推模式（实际为长轮询，broker无消息时，请求hold，5s一次check，直到设置的超时时间(默认15s)；同时broker有新消息，会notify）

通过本地缓存+数量限制进行限流
- Kafka completedFetches
- RocketMQ DefaultMQPushConsumer#ProcessQueue(TreeMap+读写锁)


## 消息存储

## 参考
https://rocketmq.apache.org/
https://www.cnblogs.com/pyng/p/13392216.html

