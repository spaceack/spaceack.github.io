# 搭建etcd分布式kv数据库集群并使用python操作

## 介绍
`etcd` 是一个分布式、可靠的键值存储数据库，用于收集分布式系统中最关键的数据。是k8s的组件之一，使用Raft一致性算法，为分布式系统而生。常用做服务发现和配置共享等应用场景。

可以使用protobuf定义的接口来进行开发，这里使用更易测试验证的第三方python库`etcd3`举例。

这里需要注意的一点是：“ `raft`算法写入时需要半数以上的节点写入成功才返回，宕机节点不超过半数则数据不丢失。” 即： 3个节点的集群，1个节点宕机无妨。但两个节点宕机后，即使另一个节点存活也无法读取数据！。四个节点集群也是两个节点宕机后，无法工作。5个节点则允许两个节点宕机。满足公式`quorum=(n+1)/2`。所以建议奇数的集群节点。因为奇数和偶数的可靠性一样，单可以节省1个节点的资源。

## 下载与安装
[etcd 下载](https://github.com/etcd-io/etcd/releases/)
[etcd  v3.4 文档](https://etcd.io/docs/v3.4/dl-build//)
### 简单测试
 ```bash
 # 运行
 ./etcd
 # etcd put 命令用于设置给定 key 的值。如果 key 已经存储其他值， put 就覆写旧值.
 ./etcdctl put greeting "Hello, etcd"
 # 通过key获取value
 ./etcdctl get greeting
 # 删除 kv
 ./etcdctl del greeting
 ```
 
### 使用docker-compose组建4节点集群。
`docker-compose.yaml` 
```yaml
version: '2'
networks:
  byfn:

services:
  etcd1:
    image: quay.io/coreos/etcd
    container_name: etcd1
    command: etcd -name etcd1 -advertise-client-urls http://0.0.0.0:2379 -listen-client-urls http://0.0.0.0:2379 -listen-peer-urls http://0.0.0.0:2380 -initial-cluster-token etcd-cluster -initial-cluster "etcd1=http://etcd1:2380,etcd2=http://etcd2:2380,etcd3=http://etcd3:2380,etcd4=http://etcd4:2380" -initial-cluster-state new
    ports:
      - 2385:2379
      - 2380
    networks:
      - byfn
 
  etcd2:
    image: quay.io/coreos/etcd
    container_name: etcd2
    command: etcd -name etcd2 -advertise-client-urls http://0.0.0.0:2379 -listen-client-urls http://0.0.0.0:2379 -listen-peer-urls http://0.0.0.0:2380 -initial-cluster-token etcd-cluster -initial-cluster "etcd1=http://etcd1:2380,etcd2=http://etcd2:2380,etcd3=http://etcd3:2380,etcd4=http://etcd4:2380" -initial-cluster-state new
    ports:
      - 2386:2379
      - 2380
    networks:
      - byfn
  
  etcd3:
    image: quay.io/coreos/etcd
    container_name: etcd3
    command: etcd -name etcd3 -advertise-client-urls http://0.0.0.0:2379 -listen-client-urls http://0.0.0.0:2379 -listen-peer-urls http://0.0.0.0:2380 -initial-cluster-token etcd-cluster -initial-cluster "etcd1=http://etcd1:2380,etcd2=http://etcd2:2380,etcd3=http://etcd3:2380,etcd4=http://etcd4:2380" -initial-cluster-state new
    ports:
      - 2387:2379
      - 2380
    networks:
      - byfn
      
  etcd4:
    image: quay.io/coreos/etcd
    container_name: etcd4
    command: etcd -name etcd4 -advertise-client-urls http://0.0.0.0:2379 -listen-client-urls http://0.0.0.0:2379 -listen-peer-urls http://0.0.0.0:2380 -initial-cluster-token etcd-cluster -initial-cluster "etcd1=http://etcd1:2380,etcd2=http://etcd2:2380,etcd3=http://etcd3:2380,etcd4=http://etcd4:2380" -initial-cluster-state new
    ports:
      - 2388:2379
      - 2380
    networks:
      - byfn
```
运行 `docker-compose up` 即可
### 使用虚拟机组建4节点集群。
4个虚拟机分别运行以下命令：
```bash
# 虚拟机1 192.168.0.156
./etcd --name infra0 --initial-advertise-peer-urls http://192.168.0.156:2380 \
  --listen-peer-urls http://192.168.0.156:2380 \
  --listen-client-urls http://192.168.0.156:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://192.168.0.156:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster infra0=http://192.168.0.156:2380,infra1=http://192.168.0.157:2380,infra2=http://192.168.0.158:2380,infra3=http://192.168.0.159:2380 \
  --initial-cluster-state new
# 虚拟机2 192.168.0.157
  ./etcd --name infra1 --initial-advertise-peer-urls http://192.168.0.157:2380 \
  --listen-peer-urls http://192.168.0.157:2380 \
  --listen-client-urls http://192.168.0.157:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://192.168.0.157:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster infra0=http://192.168.0.156:2380,infra1=http://192.168.0.157:2380,infra2=http://192.168.0.158:2380,infra3=http://192.168.0.159:2380 \
  --initial-cluster-state new
 # 虚拟机3 192.168.0.158
  etcd --name infra2 --initial-advertise-peer-urls http://192.168.0.158:2380 \
  --listen-peer-urls http://192.168.0.158:2380 \
  --listen-client-urls http://192.168.0.158:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://192.168.0.158:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster infra0=http://192.168.0.156:2380,infra1=http://192.168.0.157:2380,infra2=http://192.168.0.158:2380,infra3=http://192.168.0.159:2380 \
  --initial-cluster-state new
 # 虚拟机4 192.168.0.159
   etcd --name infra3 --initial-advertise-peer-urls http://192.168.0.159:2380 \
  --listen-peer-urls http://192.168.0.159:2380 \
  --listen-client-urls http://192.168.0.159:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://192.168.0.159:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster infra0=http://192.168.0.156:2380,infra1=http://192.168.0.157:2380,infra2=http://192.168.0.158:2380,infra3=http://192.168.0.159:2380 \
  --initial-cluster-state new
```
## python `etcd3` 库测试
安装 `pip3 install etcd3`

[etcd3文档](https://python-etcd3.readthedocs.io/en/latest/)

```python
import etcd3
# 建立连接
etcd = etcd3.client(host='127.0.0.1', port=2379)
# 添加 kv
result = etcd.put('greeting', 'hello, world!')
"""
header {
  cluster_id: 14841639068965178418
  member_id: 10276657743932975437
  revision: 16
  raft_term: 5
}
"""
# 根据 key 获得 value
result = etcd.get('greeting')    # 不存在 (None, None); 存在(b'hello, world!', <etcd3.client.KVMetadata object at 0x7ffbf072de80>)
rv = result[0].decode() if result[0] else result[0] # 取解析后的值

# 删除某个 key
result = etcd.delete('greeting') # 成功删除 True， 不存在，删除失败 False

# 返回满足前缀 key 的 value
objs = etcd.get_prefix("server1")
result = [obj[0].decode() if obj[0] else obj[0] for obj in objs]

# 获取所有的 key的 value
objs = etcd.get_all() 
result = [obj[0].decode() if obj[0] else obj[0] for obj in objs]

```

