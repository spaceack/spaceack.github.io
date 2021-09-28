# leetcode-350-两个数组的交集 II


### 题目
```
给定两个数组，编写一个函数来计算它们的交集。

示例 1：

输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
示例 2:

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
 

说明：

输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
我们可以不考虑输出结果的顺序。
进阶：

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小很多，哪种方法更优？
如果 nums2 的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/intersection-of-two-arrays-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```
#### 题解 1
```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result = []
        for i2, n2 in enumerate(nums2):
            for i1, n1 in enumerate(nums1):
                if n1 == n2:
                    result.append(n2)
                    nums1.pop(i1)
                    break
       return result          
```

### 题解2
```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result = []
        len_num1 = len(nums1)
        len_num2 = len(nums2)
        if len_num1 >= len_num2:
            for i2, n2 in enumerate(nums2):
                for i1, n1 in enumerate(nums1):
                    if n1 == n2:
                        result.append(n2)
                        nums1.pop(i1)
                        break
        else:
            for i1, n1 in enumerate(nums1):
                for i2, n2 in enumerate(nums2):
                    if n1 == n2:
                        result.append(n2)
                        nums2.pop(i2)
                        break
        return result  
```
