# Nginx配置SSL证书


### 配置示例

```
server {
    listen       443 ssl;
    server_name  wxapi.aimiter.com;
    root html;

    index index.html index.htm;
    ssl_certificate      /etc/nginx/conf.d/cert.pem;
    ssl_certificate_key  /etc/nginx/conf.d/cert.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_prefer_server_ciphers  on;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;



    location / {
        # resolver 127.0.0.1;
        proxy_pass http://127.0.0.1:8080;
        #root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

}

```
