# 蓝桥楼赛第23期-解析用户名字符串 题解


## 题目描述

在社交和即时通讯应用中，@ 字符通常用于提醒某人。例如，你在楼赛取得了好成绩，拿了奖品，激动地发送了一条微博并 @实验楼官方微博。本次挑战中，我们希望实现一个函数，能够自动提取出任意文本内容中 @ 字符后面的用户名，这对于日常使用 Python 分析社交媒体文本内容很有帮助。

补充 after_at(text) 函数中的 TODO 部分，使其实现我们需要的功能：

- 返回一段指定文本 text 中任意 @ 字符后续的正确用户名。
- 正确的用户名由连续中文字符、下划线和英文字母组成，不包含标点符号、空格和其他特殊字符。

## 要求

题目需使用 Python 3.6 完成，不能使用标准库 和 第三方库。

函数传入 text 为字符串类型，可能为空。

函数返回列表，且应按 text 字符串中的出现的正确用户名次数降序排列，次数相等无先后顺序，且不重复。

## 示例

- 示例 1:
    ```
    text = "@实验楼 @shiyanlou 我在 @ 楼赛中中奖啦"; 
    usernames = ['实验楼', 'shiyanlou']
    ```

- 示例 2:
    ```
    text = "@实验楼@shiyanlou 我在 @ 楼赛中中奖啦"; 
    usernames = ['实验楼', 'shiyanlou']

    ```

- 示例 3:
    ```
    text = "@shiyanlou @实验楼 我在 @实验楼 楼赛中中奖啦"; usernames = ['实验楼', 'shiyanlou']
    ```

- 示例 4:
    ```
    text = "@!实验楼 @shiyanlou 我在楼赛中中奖啦"; 
    usernames = ['shiyanlou']

    ```
- 示例 5:
    ```
    text = "我在楼赛中中奖啦@"; 
    usernames = []
    ```

    来源：蓝桥（实验楼）
    链接：https://www.lanqiao.cn/challenges/50212/
    

---

- 解题思路
    
    很简单的字符串模拟题。

- 题解1:

    ```python
    def after_at(text):
    """TODO
    """
    usernames = []
    def is_chinese(string):
        """
        检查整个字符串是否包含中文
        :param string: 需要检查的字符串
        :return: bool
        """
        for ch in string:
            if u'\u4e00' <= ch <= u'\u9fff':
                return True
        return False

    def is_char(char):
        if is_chinese(char):
            return True
        elif char in ("abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ_"):
            return True
        else:
            return False

    for index, t in enumerate(text):
        if t == '@':
            # 避免越界
            if index+1 < len(text):
                if is_char(text[index + 1]):
                    start = index + 1
                    end = start
                    # 对 @ 后的字符做判断， 截取用户名
                    for x in text[start:]:
                        if is_char(x):
                            end = end + 1
                            continue
                        else:
                            if text[start: end] not in usernames:
                                usernames.append(text[start: end])
                            break
    return usernames
    ```

