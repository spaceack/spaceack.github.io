# leetcode-217-存在重复元素

## 题目描述
给你一个整数数组 `nums` 。如果任一值在数组中出现 **至少两次** ，返回 `true` ；如果数组中每个元素互不相同，返回 `false` 。

## 示例 1:
```
输入: nums = [1,2,3,1]
输出: true
```
## 示例 2:
```
输入: nums = [1,2,3,4]
输出: false
```
## 示例 3:
```
输入: nums = [1,1,1,3,3,4,3,2,4,2]
输出: true
```
提示：

1 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9

来源：力扣（LeetCode）
    链接：https://leetcode-cn.com/problems/contains-duplicate/
---

- 题解1：
两种思路： 一种是排序，一种是哈希表。

    首先使用python的集合（哈希）`set()`数据结构超级简单，秒过～。 
    
    这种需要分配额外的空间用来存放哈希表。

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        scaned = set()
        for num in nums:
            if num in scaned:
                return True
            else:
                scaned.add(num)
        return False
```
