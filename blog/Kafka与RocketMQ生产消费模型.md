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

## 消息存储

## 参考
https://rocketmq.apache.org/
https://www.cnblogs.com/pyng/p/13392216.html

