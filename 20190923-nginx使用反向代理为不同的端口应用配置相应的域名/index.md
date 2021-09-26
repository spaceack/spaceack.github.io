# Nginx使用反向代理为不同的端口应用配置相应的域名


本地8081 端口有某web应用, 但只开放了80端口, 有该ip域名一枚 pet.aimiter.com, 
要通过域名pet.aimiter.com 访问本地8081端口的应用.
![config.png](config.png)

创建 配置文件: sudo cp default pet.conf, 
然后更改`server_name` 和 `proxy_pass`即可.
