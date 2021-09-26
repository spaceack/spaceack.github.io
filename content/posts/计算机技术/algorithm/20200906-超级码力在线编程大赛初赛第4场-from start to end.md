---
title: 超级码力初赛第四场 from start to end
date: 2020-09-06 21:00:00
update: 2020-09-06 21:00:00
author: Spaceack
categories: ["算法"]
tags: 
  - python
  - 算法
  - 数组
  - leetcode
  - 简单
---

## 题目描述

字符串大师赐给了你一种名为"从头到尾"的法术，其作用如下：

对一个字符串施加一次该法术的效果是：将一个字符串的第一个字母放到该字符串的结尾。例如对串"abcd"施加一次法术后可以得到串"bcda"。

现在给你两个字符串，请你判断是否可以通过任意次（可以是0次）该法术将两个字符串变得一模一样。

 1 \leq |s1|, |s2| \leq 200

字符串仅由小写字母构成


## 示例
- 示例 1:
    ```
    输入：
    "abcd"
    "bcda"
    输出：
    true
    ```
- 示例 2:
    ```
    输入：
    "abcd"
    "abdc"
    输出：
    false
    ```

    来源：九章算法
    

---
- 解题思路
    
    简单题， 一次通过。

- 题解1:

    执行用时：101

    ```python
    class Solution:
    """
    @param s1: the string 1
    @param s2: the string 2
    @return: judge can s1 change to s2
    """
    def judge(self, s1, s2):
        # write your code here
        s1 = list(s1)
        s2 = list(s2)
        if s1 == s2:
            return True
        for _ in range(len(s1)):
            start = s1.pop(0)
            s1.append(start)
            if s1 == s2:
                return True
        return False
    ```
