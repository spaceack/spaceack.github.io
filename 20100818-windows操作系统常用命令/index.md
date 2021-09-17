# windows操作系统常用命令


- 查看MAC地址
  ```
  ipconfig /all
  ```

- 查看硬盘序列号
  ```
  开始-运行-cmd-diskpart
  list disk 查看硬盘数
  select disk 0 选择0号磁盘，即当前磁盘
  detail disk 查看磁盘详细信息，第二行的磁盘ID， 即该硬盘序列号
  ```
