---
title:  Nginx配置SSL证书
author: Spaceack
date: 2023-11-09 21:00:00
update:  2023-11-09 21:00:00
categories: ["网络"]
tags: 
  - Nginx
---

1. 查看nginx启动用户和使用用户是否一致 

`ps aux | grep nginx`

2. 更改配置文件, 把 `user www-data` 改为 `root`

`sudo vim /etc/nginx/conf/nginx.conf`

3. 重启服务

`sudo service nginx restart`