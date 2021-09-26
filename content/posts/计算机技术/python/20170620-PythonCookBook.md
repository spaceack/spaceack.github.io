---
title: Python CookBook
date: 2017-06-20 08:57:27
categories: ["Python"]
update: 2020-08-20 23:00:00
author: Spaceack
tags: 
  - platform
  - lsb_release
  - python
  - passlib
  - selenium
  - 密码哈希
  - snippet
  - xpath
  - HMAC
---
#### platform
```python
import platform

# 获取操作系统的位数
print('architecture: ', platform.architecture()) # architecture:  ('64bit', 'ELF')
# 计算机类型/CPU架构
print('machine: ', platform.machine()) # machine:  x86_64 aarch64
print('system:', platform.system()) # Linux
# 获取Linux内核版本
print('platform: ', platform.platform()) # Linux-5.4.0-42-generic-x86_64-with-glibc2.29
print('version: ', platform.version()) # #46-Ubuntu SMP Fri Jul 10 00:24:02 UTC 2020
# 计算机的网络名称
print('node: ', platform.node()) # idea570
# 计算机处理器信息
print('processor', platform.processor()) # processor x86_64
print('uname: ', platform.uname()) # uname_result(system='Linux', node='idea570', release='5.4.0-42-generic', version='#46-Ubuntu SMP Fri Jul 10 00:24:02 UTC 2020', machine='x86_64', processor='x86_64')

print(platform.python_build()) # ('default', 'Jul 16 2020 14:00:26')
print(platform.python_compiler()) # GCC 9.3.0
print(platform.python_branch())
# python 解释器实现
print(platform.python_implementation()) # CPython
print(platform.python_revision())
print(platform.python_version_tuple()) # ('3', '8', '2')
```

#### lsb_release
```python
import lsb_release
lsb_info = lsb_release.get_distro_information()
print(lsb_info) # {'ID': 'Ubuntu', 'DESCRIPTION': 'Ubuntu 20.04.1 LTS', 'RELEASE': '20.04', 'CODENAME': 'focal'}
print('ID: ', lsb_info.get('ID')) # ID:  Ubuntu
print('RELEASE: ', lsb_info.get('RELEASE')) # RELEASE:  20.04
print('DESCRIPTION: ', lsb_info.get('DESCRIPTION')) # DESCRIPTION: 'Ubuntu 20.04.1 LTS
print('CODENAME: ', lsb_info.get('CODENAME')) # CODENAME:  focal
```

#### 密码哈希
  ``` python
  from passlib.apps import custom_app_context as pwd_context
  password = pwd_context.encrypt('password')
  print(password, len(password))
  # $6$rounds=656000$Q.AXo68BhIXq7LnM$bldegF6rHZ5Gi7ujPnXHOAdnAvC8srNImdIFQ9DcBWx7aF0sCWn/HryRIbyh6nuyhD3Trp.y2Sb6yVEyHsWCS1 120

  result = pwd_context.verify('password', password)
  print(result)   # True
  ```
#### selenium 登陆测试
  ```python
  import time
  from selenium import webdriver
  browser = webdriver.Firefox(executable_path='/opt/geckodriver')
  browser.get('http://127.0.0.1:8080')

  name_input = browser.find_element_by_xpath("//*[@type='text']")  # 找到用户名的框框
  pass_input = browser.find_element_by_xpath("//*[@type='password']")  # 找到输入密码的框框
  login_button = browser.find_element_by_class_name('loginButton')  # 找到登录按钮

  name_input.clear()
  name_input.send_keys('admin')  # 填写用户名
  time.sleep(0.2)
  pass_input.clear()
  pass_input.send_keys('password')  # 填写密码
  time.sleep(0.2)
  login_button.click()  # 点击登录

  c = browser.context()
  print(c)
  ```
### 删除字典值为空的元素
dict(filter(lambda x: x[1] != '', dic.items()))
```

- postgresql connect
```python
import psycopg2
conn = psycopg2.connect(database='', user='', password=, host='', port=5432)
cur = conn.cursor()
# select
query = 'select * from public.test where id=%(id)s'
query_dict = {'id': id}
cur.execute(query,query_dict)
row = cur.fetchone()
# insert
```
- pickle
```python

    with open("test.txt", 'wb+') as f:
        pickle.dump(test.txt, f)

    with open("test.txt", 'rb+') as f:
        text = pickle.load(f)
```
- PIL 显示图片流
```python
from PIL import Image
import io
stream = io.BytesIO(pic.content)
img = Image.open(stream) # img = Image.open("pic.jpg")
img.show()
img.close()
```
- socket5 proxy
```python
# socket5 proxy:
#
import copy
import socks
import socket
defaultSocket = copy.copy(socket.socket) # proxy switch
socks.setdefaultproxy(socks.PROXY_TYPE_SOCKS5, "127.0.0.1", 1080)
# socket.socket = socks.socksocket
```
- RE 
```
import re
find_string = re.findall(r"<p>(.*?)</p>" ,text_string)
```

- Requests CookieJar Dict type conversion  
```python
cookies = requests.utils.dict_from_cookiejar(r.cookies)
cookies = requests.utils.cookiejar_from_dict(cookie_dict, cookiejar=None, overwrite=True)
s = requests.Session()
s.cookies = cookies
```
```python
# 两个list 合并为一个字典
a = [1, 2, 3]
b = [a, b, c]
d = dict(zip(a, b))
# {1:a,2:b,3:c}
```
- Chrome Cookie 字符串转字典
```python
def chrome_cookie_to_dict(str):
    cookie = {}
    f = str.strip('Cookie:').strip('\n').split(';')
    for line in f:
        name, value = line.strip().split('=')
        cookie[name] = value
    return cookie
if __name__ == '__main__':
    str = 'Cookie:SESSION_COOKIE=3cab1829cea36ddbceb17f7e; JSESSIONID=E2EE5E944C1E82BE0B26613F3D9A9AAB'
    chrome_cookie_to_dict(str) # {'SESSION_COOKIE': '3cab1829cea36ddbceb17f7e', 'JSESSIONID': 'E2EE5E944C1E82BE0B26613F3D9A9AAB'}
```
-  OptionParser 命令行选项解析器, 应用场景:(命令行选项, 帮助说明)
```
from optparse import OptionParser, OptionError
usage = 'usage: %prog [options] xxx'
description = HandlerClass.__doc__
parser = OptionParser(usage=usage, description=description)
parser.add_option('-a', dest='address', help='host address', default='127.0.0.1')
parser.add_option('-p', dest='port', help='port number', default=8080)
try:
    (options, args) = parser.parse_args()
except OptionError:
    sys.exit(2)
except OptionError:
    print('No option input')
    sys.exit(2)
```
-  importlib模块动态加载 应用场景: 组件可插拔(fully pluggable)
```
import importlib
module = importlib.import_module(module_name)
```

- HMAC password generator demo
```python
import hashlib
import hmac
import base64

def hmac_password(message, salt, digestmod=hashlib.sha256):
    """
    digestmod:always_supported:'md5','sha1','sha224','sha256','sha384','sha512'.

    :param message:
    :param salt:
    :param digestmod:
    :return: hmac password
    """
    message = message.encode()
    salt = salt.encode()
    signature = base64.b64encode(hmac.new(salt, message, digestmod=digestmod)
                                 .digest())
    return signature

if __name__ == '__main__':
    m = hmac_password('secret info', 'random sring')
    print(m.decode())
```
- Stmp SSL Mail.
```python
import smtplib
import email.mime.multipart
import email.mime.text


from_mail = ''
from_mail_password = ''
to_mail = ''
mail_subject = 'test'
mail_content = '''
                    这是一封自动发送的邮件.
        '''
stmp_host = 'smtp.exmail.qq.com'
stmp_port = '465'
def SMTP_SSL():
    """
    Send mail.

    :return: (250, b'Ok')
    """
    msg = email.mime.multipart.MIMEMultipart()
    msg['from'] = from_mail
    msg['to'] = to_mail
    msg['subject'] = mail_subject
    content = mail_content
    txt = email.mime.text.MIMEText(content)
    msg.attach(txt)
    smtp = smtplib.SMTP_SSL(stmp_host, stmp_port)
    smtp.login(from_mail, from_mail_password)
    # smtp.set_debuglevel(1)
    smtp.sendmail(from_mail, to_mail, str(msg))
    return smtp.noop()

if __name__ == '__main__':
    print(SMTP_SSL())
```

#### 通过网卡名获取ip地址

```python
# 方法1： 使用 netifaces 库
# pip3 install netifaces
ip_address = ifaddresses(netcard_name)[AF_INET][0]['addr']

# 方法2：
my_addr = os.popen(
    "ifconfig | grep -A 1 %s|tail -1| awk '{print $2}'" % netcard_name).read()
ip_address = re.search(
    r'(?<![\.\d])(?:25[0-5]\.|2[0-4]\d\.|[01]?\d\d?\.)'r'{3}(?:25[0-5]|2[0-4]\d|[01]?\d\d?)(?![\.\d])',
    my_addr).group()
```
