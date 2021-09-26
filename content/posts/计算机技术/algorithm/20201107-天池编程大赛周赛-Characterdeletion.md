---
title: 天池编程大赛周赛-Character deletion
date: 2020-11-07 23:50:31
update: 2020-11-07 23:50:31
author: Spaceack
categories: ["算法"]
tags: 
  - python
  - 算法
  - 模拟
  - 简单
---

## 题目描述

Enter two strings and delete all characters in the second string from the first string


String contains spaces $1\leq len(str),len(sub) \leq 10^5$

## 示例

- 示例 1:
    ```
    Input:  str=”They are students”，sub=”aeiou”
    Output: ”Thy r stdnts”
    ```
    来源：九章算法
    链接：https://tianchi.aliyun.com/oj/141754208384739500/160296091929219251
    

---

- 解题思路
    
很简单的模拟题，由于子串可能会比较长， 直接使用 in sub 会导致超时情况， 需要先用集合操作化简。

- 题解1:
  
  正确的示范

  ```python
    class Solution:
        """
        @param str: The first string given
        @param sub: The given second string
        @return: Returns the deleted string
        """
        def CharacterDeletion(self, str, sub):
            # write your code here
            result = []
            sub = set(sub)
            for s in str:
                if s in sub:
                    pass
                else:
                    result.append(s)
            return "".join(result)
  ```