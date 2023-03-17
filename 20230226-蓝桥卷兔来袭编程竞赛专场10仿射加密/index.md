# 蓝桥 卷“兔”来袭编程竞赛专场-10仿射加密 题解

## 赛题介绍

### 挑战介绍

- 仿射密码结合了移位密码和乘数密码的特点，是一种替换密码。它是利用加密函数一个字母对一个字母的加密。加密函数是  `y=ax+b(mod m)` ，且  `a,b∈Zm`  （a、b 的值在 m 范围内），且 a、m 互质。 m 是字符集的大小，例如以 26 个字母作为编码，则  `m=26`  时，a 只能是 1、3、5、7、9、11、15、17、19、21、23、25 其中之一，b 为 0-25 之间的一个值，包括 0 和 25。
- 当  `m=26`  时字母与数字对照表如下：

-  

  | a | b | c | d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z |
  |---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 |

- 例如明文为：welcome
- 参数取值：m = 26，a = 5，b = 8  
- 加密如下：

| 明文 | w | e | l | c | o | m | e |
| ---- | ---- | ---- |---- |---- |---- |---- |---- |
| x | 22 | 4 | 11 | 2 | 14 | 12 | 4 |
| y=5x+8 | 118 | 28 | 63 | 18 | 78 | 68 | 28 |
| y mod 26 | 14 | 2 | 11 | 18 | 0 | 16 | 2 |
| 密文 | o | c | l | s | a | q | c |

### 挑战目标

- 补充文件  `affine.py`  下  `affine_encryption(text)`  函数中的 TODO 部分，使其实现我们需要的功能：
- 输入一段文本，使用  `y=5x+8(mod 26)`  函数加密，并将密文返回。
- 只对输入文本中的半角英文字符加密，其它内容保持不变。
- 将文本中的半角英文字母全部转换为小写，再进行加密计算，返回的密文中半角字母应全部是小写。
- 如果输入的文本中没有内容，则返回  `None` 。

```python
def affine_encryption(text: str) -> str:
    """TODO
    """
    encryption_text : str = ''
    return encryption_text
```

### 挑战要求

- 题目需使用 Python3 完成，不能使用标准库和第三方库。
- 函数传入的 text 为字符串类型，可能为空、 `None`  等值。
- 不得修改文件路径、文件名  `affine.py`  以及函数名  `affine_encryption(text)` 。
- 请只保留文件  `affine.py`  及文件中函数，不要添加测试或执行代码，避免检测时出错。
- 线上环境调试代码时，请使用  `python3 affine.py`  命令调用 Python3。

### 参考样例

```python
# 样例 1
text = "welcome"; encryption_text = "oclsaqc"
# 样例 2
text = "welcome 你好"; encryption_text = "oclsaqc 你好"
# 样例 3
text = " welcome"; encryption_text = " oclsaqc"
# 样例 4
text = " Ｑｒwe"; encryption_text = "Ｑｒoc"
# 样例 5
text = None; encryption_text = None
```

注意：最终实现效果以完全满足要求为准，而不是仅满足如上样例。

---

## 题解

### 解题思路

1. 要注意对传入参数类型与长度检查。
2. 使用`index`获取字母的下标，以对应字母对照表
3. 最后使用`join`方法将列表拼接为字符串返回即可。

```python
def affine_encryption(text: str) -> str:
    """TODO
    """
    def c(x):
        y = 5 * x + 8
        return y % 26

    upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    low = "abcdefghijklmnopqrstuvwxyz"
    lower_text = []
    if not isinstance(text, str):
        return None
    if text == "" or text == None:
        return None
    
    for t in text:
        if t in upper:
            num = c(low.index(t.lower()))
            lower_text.append(low[num])
        else:
            if t in low:
                num = c(low.index(t))
                lower_text.append(low[num])
            else:
                lower_text.append(t)

    encryption_text : str = ''.join(lower_text)
    return encryption_text
```

---

题目来源：蓝桥 [仿射加密](https://www.lanqiao.cn/problems/2404/learning/?contest_id=83)

