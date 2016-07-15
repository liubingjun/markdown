---
title: Kafka随笔
date: 2016-06-01 11:38:42
tags: 服务器
---
# Kafka笔记
## 命令
启动命令

```
bin/zookeeper-server-start.sh config/zookeeper.properties &
bin/kafka-server-start.sh config/server.properties
```
创建topic

```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 4 --topic test
```

## 监控工具
1.kafka manager [https://github.com/yahoo/kafka-manager](https://github.com/yahoo/kafka-manager)
雅虎的开源工具

2.Kafka web console[ https://github.com/claudemamo/kafka-web-console]( https://github.com/claudemamo/kafka-web-console)
尚不稳定、功能还可以

3、KafkaOffsetMonitor[https://github.com/quantifind/KafkaOffsetMonitor](https://github.com/quantifind/KafkaOffsetMonitor)
功能比较简单，启动命令：

```
java -cp KafkaOffsetMonitor-assembly-0.2.1.jar com.quantifind.kafka.offsetapp.OffsetGetterWeb --zk 192.168.57.207:2181 --port 8089 --refresh 10.seconds --retain 1.days
```

## 资源链接
>[Kafka官方文档](http://kafka.apache.org/documentation.html)

>[国内红磊Kafka博客](http://blog.csdn.net/honglei915/article/category/2383433)

The broker.id property is the unique and permanent name of each node in the cluster. We have to override the port and log directory only because we are running these all on the same machine and we want to keep the brokers from all trying to register on the same port or overwrite each others data.