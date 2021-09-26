# Python3编码转换

没有什么编码是不能转的

```python
import hashlib
import base64
# string to md5
input_text = "我能吞下玻璃而不伤身体"
md5_string = hashlib.md5(input_text.encode(encoding='utf8')).hexdigest()
# 2e536f0d3a95e676e30afb2b511c6fe2

# string to base64
base64_string = base64.b64encode(input_text.encode('utf-8')).decode('utf-8')
# 5oiR6IO95ZCe5LiL546755KD6ICM5LiN5Lyk6Lqr5L2T 

# base64 to string
output_text = base64.b64decode(base64_string).decode(encoding='utf8')
# 我能吞下玻璃而不伤身体

# URL Decode quote/unquote
import urllib.parse
result = urllib.parse.unquote("%E6%88%91%E8%83%BD%E5%90%9E%E4%B8%8B%E7%8E%BB%E7%92%83%E8%80%8C%E4%B8%8D%E4%BC%A4%E8%BA%AB%E4%BD%93")
#  我能吞下玻璃而不伤身体
result = urllib.parse.quote(input_text)
#%E6%88%91%E8%83%BD%E5%90%9E%E4%B8%8B%E7%8E%BB%E7%92%83%E8%80%8C%E4%B8%8D%E4%BC%A4%E8%BA%AB%E4%BD%93

# string to hex
bytes_string = input_text.encode()
bytes_string = b'\xe6\x88\x91\xe8\x83\xbd\xe5\x90\x9e\xe4\xb8\x8b\xe7\x8e\xbb\xe7\x92\x83\xe8\x80\x8c\xe4\xb8\x8d\xe4\xbc\xa4\xe8\xba\xab\xe4\xbd\x93'
hex_str = bytes_string.hex()
# e68891e883bde5909ee4b88be78ebbe79283e8808ce4b88de4bca4e8baabe4bd93

# hex to string
text = bytes.fromhex(hex_str).decode()
# 我能吞下玻璃而不伤身体

```

### ipv4字符串与数字转换
  ```python3
  import socket
  import struct
  ip = '127.0.0.1'
  int_ip = struct.unpack('!I', socket.inet_aton(ip))[0]
  print(int_ip)
  str_ip = socket.inet_ntoa(struct.pack('!I', int_ip))
  print(str_ip)
  ```



