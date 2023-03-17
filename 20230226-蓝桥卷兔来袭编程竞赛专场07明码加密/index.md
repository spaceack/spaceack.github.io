# 蓝桥 卷“兔”来袭编程竞赛专场-07明码加密 题解

## 赛题介绍

### 挑战介绍

- 清末，电报技术进入中国。上海大北水线电报公司在 1871 年选用了六千八百九十七个汉字，代以四码数字，编写成了中国最早的电报明码本。为了传输的内容可以保密，又设计出了将明码本加密的方法，于是就有了比较复杂的密码。后来，这种加密技术被应用在军事和商业情报机构中。  
- 加密的具体方法是：例如“布”这个字的电报电码为 1530，加密和减密的钥匙均为 9853。先用明码的四个数字分别与加密钥匙的四个数字相加，例如第一位数相加  `1 + 9 = 10` ，凡 10 都作  `0` ；第二位数相加  `5 + 8 = 13` ，隐去 10，只作  `3` ；第三位数相加  `3 + 5 = 8` ；第四位数相加（如果是 0，则作 10 ）  `10 + 3 = 13` ，隐去 10，只作  `3` 。那么“布”的明码 1530 就成了密码 0383。解密时相反，使用密码减去解密钥匙的四个数字，即可获得明码，从而找到对应的文字。  

### 挑战目标

补充文件  `plain_code.py`  下  `plain_code_encryption(numb)`  函数中的 TODO 部分，使其实现我们需要的功能：

- 输入一个 0 - 9999 （包含 0 和 9999）之间的任意数，然后加上 9853，相加之后得到一个 4 位数（str 格式）并返回。
- 如果传入的数字不足 4 位，则缺失位置以 0 填补。例如输入数字为 32，则需要当作 0032。
- 相同位置的数字进行相加，如果相加之后大于等于 10，则隐去 10，只保留个位位置的数字。
- 如果输入的数字不在 0 - 9999 之间，则返回  `None` 。

```python
def plain_code_encryption(numb: int) -> str:
    """TODO
    """
    encryption_text : str = ''
    return encryption_text
```

### 挑战要求

- 题目需使用 Python3 完成，不能使用标准库和第三方库。
- 函数传入 numb 为整型类型，可能为 -1、11111 等值。
- 不得修改文件路径、文件名  `plain_code.py`  以及函数名 `plain_code_encryption(numb)` 。
- 请只保留文件  `plain_code.py`  及文件中函数，不要添加测试或执行代码，避免检测时出错。
- 线上环境调试代码时，请使用  `python3 plain_code.py`  命令调用 Python3。

### 参考样例

```python
# 样例 1
numb = 1530; encryption_text = "0383"
# 样例 2
numb = 0; encryption_text = "9853"
# 样例 3
numb = 12345; encryption_text = None
```

注意：最终实现效果以完全满足要求为准，而不是仅满足如上样例。

---

## 题解

### 解题思路

简单的模拟题

1. 要注意对传入参数长度的检查。
2. 利用模运算符进行取余运算获取余数。
3. 最后使用`join`方法将列表拼接为字符串返回即可。

```python
def plain_code_encryption(numb: int) -> str:
    """TODO
    """
    result = []
    crypt = "9853"
    if not 0 <= int(numb) <=9999:
        return None
    str_numb = str(numb)
    str_numb = str_numb.rjust(4,'0')
    for index, value in enumerate(str_numb):
        if (int(value) + int(crypt[index])) < 10:
            result.append(str(int(value) + int(crypt[index])))
        else:
            result.append(str((int(value) + int(crypt[index])) % 10))
    encryption_text : str = ''.join(result)
    return encryption_text
```

---

题目来源：蓝桥 [明码加密](https://www.lanqiao.cn/problems/2401/learning/?contest_id=83)

