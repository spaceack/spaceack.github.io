---
title: 使用Mysql实现消息队列
date: 2021-09-29 21:00:00
update: 2021-09-29 21:00:00
author: Spaceack
categories: ["消息队列"]
tags: 
  - MQ
  - mysql
weight: 1
---

![罗小黑羽毛球趣图](1.gif)


实现起来就是 **消息** 带 `状态` 和 `版本号` 字段。

![mq表结构](2.png)


更新时用 `版本号` 做乐观锁。操作逻辑就是个状态机。

```UPDATE mq SET mq.status=new_status mq.version = mq.version + 1 WHERE mq.version = old_version```

## 实现
### mysql mq 表结构设计

```sql
CREATE TABLE `mq` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `msg` varchar(1024) DEFAULT NULL,
  `status` varchar(100) DEFAULT 'ready',
  `version` bigint(20) unsigned DEFAULT 0,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8mb4;
```

### 生产者
测试，向队列插入两条消息。
```sql
insert into mq(msg) values('第一条消息 hello world! 1024');
insert into mq(msg) values('第二条消息 欢迎访问 https://spaceack.com');
```
![初始数据](3.png)
### 消费者
- 获取队列中的消息， 此时不会改变队列（mq表）中的数据。
  ```sql
  select * from mq where status='ready' limit 1;
  ```
  ![初始数据](4.png)

- 消息确认

  ```sql
  update mq set mq.status = 'ack', mq.version = mq.version + 1 WHERE mq.version = {query_version} and id = {query_id}
  ```

  确认后的状态：
  
  ![初始数据](5.png)

再次获取数据仅能获取第二条数据。

![初始数据](6.png)

---

这样的一个好处就是消息都是可见的。 可以直接查数据库。

不用加悲观锁，因为update成功后就带锁。其它同版本的更新都会失败。

还有个好处就是减少组件依赖。 简单的服务数据库就能搞定。 不用再起个rabbitmq服务啥的。 节约运维成本。

**版本号** 另一个小作用：InnoDB如果更新语句没有改变任何字段值时，影响行数会返回0.那么是没找到记录还是没改变值的0呢？前者是bug， 后者是正常情况但区分不了。加版本号后每次修改+1，影响行数一定不为0。

---
隐藏福利：生产者自动批量测试脚本：
```python
import random
import subprocess

def runcmd(command):
    ret = subprocess.run(command,shell=True,stdout=subprocess.PIPE,stderr=subprocess.PIPE,encoding="utf-8",timeout=1)
    if ret.returncode == 0:
        # print("success:",ret, ret.stdout)
        return ret.stdout
    else:
        print("error:",ret)


def producer():
    sql = "\"insert into mq(msg) values('%s');\"" % (str(random.randint(1,99999)))
    cmd = """mysql -uroot -ppassword test -e %s """ % (sql)
    runcmd(cmd)

```
