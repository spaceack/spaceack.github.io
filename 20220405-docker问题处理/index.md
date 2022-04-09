# Docker问题处理

### x509: certificate signed by unknown authority
`docker pull` 或 `docker login 192.1.11.18` 报错
```
Error response from daemon: Get "https://192.1.11.18/v2/": x509: certificate signed by unknown authority
```
解决方法：
- 在`/etc/docker/daemon.json/daemon.json` 添加 `{insecure-registries":["192.1.11.18"]}`, `sudo systemctl restart docker`

- MacOS Docker Desktop -> Preferences -> Docker Engine

    Configure the Docker daemon by typing a json Docker daemon configuration file.


  添加 `"insecure-registries":["192.1.11.18"]`
    ```
    {
    "experimental": false,
    "builder": {
        "gc": {
        "defaultKeepStorage": "20GB",
        "enabled": true
        }
    },
    "features": {
        "buildkit": true
    },
    "insecure-registries":["10.1.110.188"]
    }
    ```

