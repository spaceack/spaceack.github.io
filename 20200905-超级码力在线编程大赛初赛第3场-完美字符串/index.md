# 超级码力初赛第三场 完美字符串


## 题目描述

定义若一个字符串的每个字符均为'1'，则该字符串称为完美字符串。给定一个只由'0'和'1'组成的字符串s和一个整数k。你可以对字符串进行任意次以下操作

选择字符串的一个区间长度不超过k的区间[l, r]，将区间内的所有'0'修改成'1'，将区间内所有的'1'修改成'0'。

你最少需要多少次操作，可以将字符串s修改成一个完美字符串

1<=len(s)<=100,000
1<=k<=100,000

## 示例
- 示例 1:
    ```
    输入: 
    s="10101"
    k=2
    输出: 
    2
    解释: 
    1. 修改 [1,2] ==> "11001"
    2. 修改 [2,3] ==> "11111"

    ```
- 示例 2:
    ```
    输入: 
    s="00000"
    k=3
    输出: 
    2
    解释:
    1. 修改 [0,2] ==> "11100"
    2. 修改 [3,4] ==> "11111"

    ```

    来源：九章算法
    

---
- 解题思路
    
    简单的模拟，要注意边界条件，一次通过。

- 题解1:

    执行用时：402

    ```python
    class Solution:
        """
        @param s: string need to be transformed
        @param k: minimum char can be transformed in one operation
        @return: minimum times of transforming all char into '1'
        """
        def perfectString(self, s, k):
            # Write your code here.
            result = 0
            s_len = len(s)
            s_list = list(s)
            for index, s in enumerate(s_list):
                if s == 1:
                    continue
                if s == '0':
                    start_at = index
                    end_at = index
                    if end_at+1 <= s_len:
                        while s_list[end_at] == '0' and end_at - start_at < k:
                            end_at = end_at + 1
                            if end_at > s_len -1:
                                break
                        for x in range(start_at, end_at):
                            s_list[x] = '1'

                        result = result + 1
            return result
    ```

