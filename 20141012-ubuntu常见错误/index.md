# Linux 错误修复笔记


## apt

- 错误 ："subprocess installed post-installation script returned error exit status 1"
- 故障排除：

```bash

apt-get autoclean
apt-get autoremove
apt-get update
apt-get upgrade
```
