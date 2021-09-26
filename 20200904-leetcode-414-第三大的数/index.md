# leetcode-414-第三大的数


## 题目描述

给定一个非空数组，返回此数组中第三大的数。如果不存在，则返回数组中最大的数。要求算法时间复杂度必须是O(n)。


## 示例
- 示例 1:
    ```
    输入: [3, 2, 1]

    输出: 1

    解释: 第三大的数是 1.
    ```
- 示例 2:
    ```
    输入: [1, 2]

    输出: 2

    解释: 第三大的数不存在, 所以返回最大的数 2 .
    ```
- 示例 3:
    ```
    输入: [2, 2, 3, 1]

    输出: 1

    解释: 注意，要求返回第三大的数，是指第三大且唯一出现的数。
    存在两个值为2的数，它们都排第二。
    ```

    来源：力扣（LeetCode）
    链接：https://leetcode-cn.com/problems/third-maximum-number/
    

---
- 解题思路

  `题解1` 首先用苯办法，定义一个最大长度为三的有序列表，依次从 nums 列表中取值，插入该有序列表。

    优点是节约内存， 缺点是判断比较麻烦。
    
  `题解2` 超级简洁，执行速度快。利用 python 的 `heapq` 模块的 `nlargest()` 方法。它会查找最大的n个数， 本质是堆排序。所以输出结果是无序的， 要再用 `sort` 进行排序。 

- 题解1:

    执行用时：80 ms, 在所有 Python3 提交中击败了21.76%的用户
    
    内存消耗：14.4 MB, 在所有 Python3 提交中击败了92.21%的用户

    ```python
    from typing import List
    class Solution:
        def thirdMax(self, nums: List[int]) -> int:
            # 默认右边最大
            th = []
            while nums:
                num = nums.pop()
                if num in th:
                    continue
                if not th:
                    th.append(num)
                if len(th) == 1:
                    if num > th[0]:
                        th.append(num)
                    elif num < th[0]:
                        th.insert(0, num)
                    # 这里需要注意， 第二次因为没有continue.会执行到下面的判断，低级错误。
                    continue
                if len(th) == 2:
                    if num > th[1]:
                        th.append(num)
                    elif num < th[0]:
                        th.insert(0, num)
                    elif num > th[0]:
                        th.insert(1, num)
                    continue
                if len(th) == 3:
                    if num > th[2]:
                        th.append(num)
                        del th[0]
                    elif num < th[0]:
                        continue
                    elif num > th[0] and num < th[1]:
                        del th[0]
                        th.insert(0, num)
                    elif num > th[1] and num < th[2]:
                        del th[0]
                        th.insert(1, num)
                    continue
                    # 这里需要注意， 第一次提交下标写的是2，没有考虑到1的情况，要用反向索引-1
            return th[-1] if len(th) < 3 else th[0]
    ```
- 题解2
 执行用时：32 ms, 在所有 Python3 提交中击败了99.17%的用户
 内存消耗：15.9 MB, 在所有 Python3 提交中击败了16.91%的用户
	```python
	import heapq
	class Solution:
	    def thirdMax(self, nums: List[int]) -> int:
	        tmp = heapq.nlargest(3, set(nums))
	        tmp.sort(reverse=True)
	        return tmp[0] if len(tmp) < 3 else tmp[2]
	```
