---
title: Python urllib3
date: 2019-06-07 21:00:00
update: 2019-06-07 21:00:00
author: Spaceack
categories: ["Python"]
tags: 
  - python
  - urllib3
---

### 发送POST请求，内容为json格式。并忽略证书验证。

```python
import json
import urllib3
urllib3.disable_warnings() # 屏蔽警告忽略证书的警告。
# cert_reqs='CERT_NONE' 忽略https证书验证
http = urllib3.PoolManager(cert_reqs='CERT_NONE')
encode_data = json.dumps({"name":"spaceack"}).encode("utf-8")
url = "https://127.0.0.1:5000/api/v1/hello"
req = http.request('POST',
                    url=url, 
                    body=encode_data,
                    headers={'Content-Type': 'application/json'},
                    timeout=10,
                    )
result = req.data.decode('utf-8') # 获取响应的数据。
```