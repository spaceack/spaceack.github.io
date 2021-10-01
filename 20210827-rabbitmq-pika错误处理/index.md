# RabbitMQ pika错误处理 delivery acknowledgement on channel 1 timed out


- AMQPChannelError 异常捕获信息：
`(406, 'PRECONDITION_FAILED - delivery acknowledgement on channel 1 timed out. Timeout value used: 1800000 ms. This timeout value can be configured, see consumers doc guide to learn more')`

其他人也与到过这个[问题](https://groups.google.com/g/rabbitmq-users/c/X-x3DKVJb8k)

按照官方文档配置超时参数后问题解决。

` pika.ConnectionParameters(heartbeat=600, blocked_connection_timeout=300)`

[DOC: heartbeat_and_blocked_timeouts](https://pika.readthedocs.io/en/stable/examples/heartbeat_and_blocked_timeouts.html)
