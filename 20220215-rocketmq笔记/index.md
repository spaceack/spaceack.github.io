# RocketMQ笔记

[rocketmq](https://rocketmq.apache.org/)

[Apache RocketMQ开发者指南-官方](https://github.com/apache/rocketmq/tree/rocketmq-all-4.5.1/docs/cn)

[Apache RocketMQ开发者指南](https://www.itmuch.com/books/rocketmq/)

[rocketmq-externals](https://github.com/apache/rocketmq-externals.git)

## 安装运行
`cd /opt/rocketmq/bin`

启动mqnamesrv `nohup sh mqnamesrv &`

查看名字服务日志 `tail -f ~/logs/rocketmqlogs/namesrv.log`

启动 broker 并允许自动创建topic `nohup sh mqbroker -n localhost:9876 autoCreateTopicEnable=true &`

查看 broker 日志`tail -f ~/logs/rocketmqlogs/broker.log`

停止broker `sh mqshutdown broker`

停止mqnamesrv `sh mqshutdown namesrv`


## 基础

`Producer` 生产消息
`Consumer` 消费消息
`Broker` 存储消息

## GUI可视化管理控制台 rocketmq-console-ng

