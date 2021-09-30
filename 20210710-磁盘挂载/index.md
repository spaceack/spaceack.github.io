# Linux 磁盘挂载

### 查看磁盘

```
sudo fdisk -l
sudo lsblk
sudo mkfs -t ext4 /dev/mmcblk1 # 格式化
sudo mount /dev/mmcblk1 /home/root/data/
```

### 卸载挂载
```
sudo umount /dev/mmcblk1
```

### 自动挂载
```
sudo blkid # 获取UUID
sudo vim /etc/fstab
UUID=6b7eaf8d-0b66-6e26-bdff-fa63f7dd1126 /u01 ext4 defaults 1 1
```

