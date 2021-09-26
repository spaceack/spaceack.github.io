# RCA-MongoDB数据写入失败


## 问题现象

程序崩溃，提示MongoDB写入失败，无法再连起。

## 分析原因
1.首先想到分析mongoDB日志记录
通过 ```cat /etc/mongod.conf``` 找到日志所在目录 ```/var/log/mongodb/mongod.log```
```
2018-11-07T16:50:44.165+0800  W FTDC     [ftdc] Uncaught exception in 'FileStreamFailed: Failed to write to interim file buffer for full-time diagnostic data capture: /var/lib/mongo/diagnostic.data/metrics.interim.temp' in full-time diagnostic data capture subsystem. Shutting down the full-time diagnostic data capture subsystem.
2018-11-07T16:51:30.913+0800 E STORAGE  [WTCheckpointThread] WiredTiger error (28)：handle-write: pwrite: failed to write 4096 bytes at offset 1486848: No space left on device
2018-11-07T16:51:30.914+0800 E STORAGE  [WTCheckpointThread] WiredTiger error (28): fatal checkpoint failure: No space left on device
2018-11-07T16:51:30.914+0800 E STORAGE  [WTCheckpointThread] WiredTiger error (-31804)  WT_SESSION.checkpoint: the process must exit and restart: WT
```
日志反馈的信息很明确，“磁盘已被写满啦！”， 但是很奇怪，写入量并不大，且只有唯一任务在执行，写满是不可能的。
可能想到的问题是蠕虫病毒，或是由程序递归，死循环等造成的错误数据写入。

2.那么现在的目标就是找到占用的文件，我现在希望这是一个大文件，若干个碎片文件查找起来会很痛苦（虽然也可通过写入时间搜索）。
幸好所在磁盘分区不大，首先进入数据目录所在分区根目录，查找大于100M的单文件 ```find . -type f -size +100M```。很快定位到一个16G的日志文件!验证了之前的猜想。

3.为什么会形成如此大的日志文件？？？ 初步分析是由一个第三方库写入的。
## 解决方案

1. 为了快速释放服务器资源并启动服务，初步方案是删除日志文件，注释掉日志记录代码，代码线下再做检查。
2. 重启mongoDB, 服务恢复。

## 经验总结
虽然问题不复杂，也很快得以解决。但也有许多地方值得注意:

1. 不要完全信任第三方库。尤其是很冷门的库。要做测试审查。
2. 数据写入到系统分区，系统分区写满严重影响其它程序执行，数据写入，非常危险！。应保持系统分区独立性。所有数据写入包括日志文件应存入单独的数据盘。
3. 系统负载报警机制也很重要，若线上同时有重要的实时任务运行，后果很严重。发现报警可以提前做处理，避免资源超载。

