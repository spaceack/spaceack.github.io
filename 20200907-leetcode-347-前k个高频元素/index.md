# leetcode-347-前K个高频元素


## 题目描述

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

提示：

你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
你可以按任意顺序返回答案。


## 示例
- 示例 1:
    ```
    输入: nums = [1,1,1,2,2,3], k = 2
    输出: [1,2]
    ```
- 示例 2:
    ```
    输入: nums = [1], k = 1
    输出: [1]
    ```
- 示例 3:
    ```
    nums = [4,1,-1,2,-1,2,3], k = 2
    输出: [-1,2]
    ```

    来源：力扣（LeetCode）
    链接：https://leetcode-cn.com/problems/robot-return-to-origin
    

---
- 解题思路
    
    1. python对字典的操作要熟练.
    2. 基本思路是先把元素和元素个数存入字典, 然后反转key-value.
    3. 因为value 有重复的情况, 所以把重复对应的key以List形式作为值.
    4. 再次就是对字典按键排序, 值列表合并, 列表二维转一维.

- 题解1:

    执行用时：44 ms, 在所有 Python3 提交中击败了94.56%的用户
    
    内存消耗：16.4 MB, 在所有 Python3 提交中击败了93.97%的用户

    ```python
    from typing import List, Dict, Iterable
    import operator
    from functools import reduce

    class Solution:
        def topKFrequent(self, nums: List[int], k: int) -> List[int]:
            rd = {} # type: Dict[int, int]
            cd = {} # type: Dict[int, List[int]]
            # 把元素和元素个数对存入字典
            for n in nums:
                if n not in rd:
                    rd[n] = 1
                else:
                    rd[n] = rd[n] + 1
            # 反转key-value
            for x in rd:
                if rd[x] not in cd:
                    cd[rd[x]] = [x]
                else:
                    cd.setdefault(rd[x], []).append(x)
            # 字典按键排序
            cd_reverse = sorted(cd.keys(), reverse=True)
            # 值(列表)合并
            rl = [cd[x] for x in cd_reverse] # type: List[List[int]]
            # 二维数组转一维
            result = reduce(operator.add, rl) #type: List[int]
            return result[0:k]
    ```

