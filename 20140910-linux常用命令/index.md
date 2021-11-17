# Linux 常用命令

##  系统篇
```
# 查看LBS 发行版本
cat /etc/lsb-release
```
```
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04.5 LTS"

```
```bash
# 查看系统有哪些shell
cat /etc/shell
# 创建用户spaceack; 生成home目录 /home/spaceack ; 指定默认使用zsh
sudo adduser --home /home/spaceack --shell /usr/bin/zsh spaceack
# 用户给予sudo权限, 直接加入sudo组
sudo usermod -aG sudo spaceack
# 其它实现方法
sudo useradd -d /home/spaceack -s /bash/zsh spaceack

```

### CPU
```bash
lscpu
cat /proc/cpuinfo
# 查看CPU架构
uname -a | awk '{print $12}'
```
### 内存

```bash 
# 查看主板内存插槽数及使用情况
dmidecode|grep -P -A 5 "Memory\s+Device" | grep Size|grep -v Range

# 查看主板支持最大内存
sudo dmidecode | grep -P 'Maximum\s+Capacity'

# 查看内存频率
sudo dmidecode | grep -A16 "Memory Device"|grep 'Speed'

# 查看内存及交换空间使用情况
# -h 以 Gibibytes为单位查看.
free -m

```
#### 交换分区
查看类型为 swap 的 交换分区名称

`sudo fdisk -l | grep swap`

查看已存在的交换分区名称

`swapon -s`

临时关闭交换分区

关闭交换分区时, 会把交换分区的数据转移到内存(需确保可用内存 大于 交换分区的已用空间.)

`sudo swapoff /dev/sda5`
开启交换分区

`sudo swapon /dev/sda5`

### 硬件设备
```bash
# 查看硬件设备
lshw
# PCI 设备列表
lspci
# 列出区块设备
lsblk
# 列出分区表
fdisk -l

```
### 网络
```bash
# 改ip
sudo ifconfig down
sudo ifconfig eth0 192.168.0.200 netmask 255.255.255.0 up
# 免sudo密码交互
echo 'password' | sudo -S ifconfig eth0 192.168.0.200 netmask 255.255.255.0 up
sudo /etc/init.d/networking restart
```
## 传输篇

### scp

  ```bash
  # 使用scp 传输文件 将本地文件data.zip 传到服务器 /data 目录 -
  # P ssh端口号
  scp -P 22 /data/data.zip root@spaceack.com:/data

  # 使用scp 传输文件 将远程目录 remote_path 传到本地当前目录
  scp   remote_username@remote_ip:/remote_path/*  .
  ```

### 使用rsync断点续传备份包含大量数据的资源目录!

  ```bash
  rsync -vrtP --rsh='ssh -p 10050' root@66.6.66.666:/data/* /data/backup
  ```

  -v, --verbose 详细模式输出.

  -r, --recursive 子目录递归处理.

  -t, --times 保持文件时间信息.

  -P,--partial 断点续传, --progress 显示传输过程.

  --rsh=COMMAND 指定使用rsh、ssh方式进行数据同步, rsh为明文传输.

  -u, 只进行更新，防止本地新文件被重写(不覆盖更新的文件).

  --ignore-existing, 跳过接收端已存在的文件,目录增量备份会用到.


## 其它
```bash
# 查看主机 ssh 连接数
w | grep pts |wc -l

# 文件批量重命名
apt install rename 

# 制作U盘启动盘（Centos7）
pv -cN source < CentOS-7-x86_64-DVD-1804.iso | sudo dd of=/dev/sdc
```
### 选择时区 tzselect
```bash
tzselect
# 查看时间
date
```
