# 蓝桥 卷“兔”来袭编程竞赛专场-06姜子牙阴书加密 题解

## 赛题介绍

### 挑战介绍

姜子牙阴书密码是将一封完整的书信分割成三份，然后由三个送信者各送一份，收信人收到三份书信后再合并成一封完整的情报。如此，即使某个信使被敌军抓获，敌军也不会获得完整的情报。

但是如果三个送信者被同时抓获，敌军还是可以获取完整的情报。因此在古代匮乏的条件之下，可以通过增加送信者的数量将书信分割成更多份传递，减少情报泄密的机率。

### 挑战目标

补充文件 `yin_book.py` 下 `yin_book_encryption(text)` 函数中的 TODO 部分，使其实现我们需要的功能：

- 输入一段文本内容，将内容分成若干份，第一份内容字数为 `1`，第二份内容字数为 `1 + 2`，第三份内容字数为 `1 + 2 + 3`，第 n 份内容字数为 `1 + 2 + 3 + ... + n`，以此类推，至止内容被分完。最后按照划分的顺序，以列表的形式将内容返回。
- 如果输入的文本没有内容，则返回 `None`。
- 一个空格也占据一个文字。

```python
def yin_book_encryption(text: str) -> list:
    """TODO
    """
    encryption_text : list = []
    return encryption_text
```

### 挑战要求

- 题目需使用 Python3 完成，不能使用标准库和第三方库。  
- 函数传入的 text 为字符串类型，可能为空、 `None`  等值。  
- 不得修改文件路径、文件名  `yin_book.py`  以及函数名  `yin_book_encryption(text)` 。  
- 请只保留文件  `yin_book.py`  及文件中函数，不要添加测试或执行代码，避免检测时出错。  
- 线上环境调试代码时，请使用  `python3 yin_book.py`  命令调用 Python3。  

### 参考样例

```python
# 样例 1
text = "姜子牙阴书加密"; encryption_text = ["姜", "子牙阴", "书加密"]
# 样例 2
text = "姜子牙阴书 加密"; encryption_text = ["姜", "子牙阴", " 书加密"]
# 样例 3
text = "None"; encryption_text = ["N", "one"]
# 样例 4
text = ''; encryption_text = None
```

注意：最终实现效果以完全满足要求为准，而不是仅满足如上样例。

---

## 题解

### 解题思路

简单的模拟题

1. 要注意对传入参数类型的检查，包括空字符串等情形。
2. 构造等差数列子函数。
3. 注意份数与等差数列的和的关系，及边界条件。
4. 最后使用`join`方法将列表拼接为字符串返回即可。

```python
def yin_book_encryption(text: str) -> list:
    """TODO
    """
    if not isinstance(text, str):
        return None
    if text == "":
        return None
    encryption_text: list = []
    text_len = len(text)
    n = 1
    sum = 0

    def seqsum(n):
        return int(n * (n+1)/2)

    while sum < text_len:
        word_num = seqsum(n)
        start = sum
        n = n + 1

        sum = sum + word_num
        encryption_text.append(text[start:sum])

    return encryption_text
```

---

题目来源：蓝桥 [姜子牙阴书加密](https://www.lanqiao.cn/problems/2400/learning/?contest_id=83)

