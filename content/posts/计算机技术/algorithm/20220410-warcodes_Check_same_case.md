---
title: Check same case
date: 2022-04-10 23:01:18
update: 2022-04-10 23:01:18
author: Spaceack
categories: ["算法"]
tags: 
  - python
  - 算法
  - warcodes
  - 简单
  - 字符串
draft: true
---

### 题目
```
编写一个函数来检查两个给定的字符是否大小写相同。

如果任何字符不是字母，则返回-1
如果两个字符大小写相同，则返回1
如果两个字符都是字母且大小写不同，则返回0
例子
'a'并'g'返回1

'A'并'C'返回1

'b'并'G'返回0

'B'并'g'返回0

'0'并'?'返回-1
```
#### 题解 1
```python
def same_case(a, b): 
    # your code here
    if a.isalpha() and b.isalpha():
        if a.islower() and b.islower() or a.isupper() and b.isupper():
            return 1
        else:
            return 0
    return -1


class test(object):
    def assert_equals(a, b):
        if a == b:
            print(True)
            return True
        else:
            print(False, a, b)
            return False

test.assert_equals(same_case('C', 'B'), 1)
test.assert_equals(same_case('b', 'a'), 1)
test.assert_equals(same_case('d', 'd'), 1)
test.assert_equals(same_case('A', 's'), 0)
test.assert_equals(same_case('c', 'B'), 0)
test.assert_equals(same_case('b', 'Z'), 0)
test.assert_equals(same_case('\t', 'Z'), -1)
test.assert_equals(same_case('H', ':'), -1)

```

### 题解2 最佳实践
```python
def same_case(a, b):
    return a.isupper() == b.isupper() if a.isalpha() and b.isalpha() else -1
```