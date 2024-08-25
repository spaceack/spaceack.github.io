# leetcode-1046-最后一块石头的重量 题解


## 题目描述

有一堆石头，每块石头的重量都是正整数。
每一回合，从中选出两块 **最重的** 石头，然后将它们一起粉碎。假设石头的重量分别为 `x` 和 `y`，且 `x <= y`。那么粉碎的可能结果如下：
- 如果 `x == y`，那么两块石头都会被完全粉碎；
- 如果 `x != y`，那么重量为 `x` 的石头将会完全粉碎，而重量为 `y` 的石头新重量为 `y-x`。
最后，最多只会剩下一块石头。返回此石头的重量。如果没有石头剩下，就返回`0`。


## 示例
- 示例 1:
    ```
    输入：[2,7,4,1,8,1]
    输出：1
    解释：
    先选出 7 和 8，得到 1，所以数组转换为 [2,4,1,1,1]，
    再选出 2 和 4，得到 2，所以数组转换为 [2,1,1,1]，
    接着是 2 和 1，得到 1，所以数组转换为 [1,1,1]，
    最后选出 1 和 1，得到 0，最终数组转换为 [1]，这就是最后剩下那块石头的重量。
    ```
## 提示：
1. 1 <= stones.length <= 30
2. 1 <= stones[i] <= 1000

    来源：力扣（LeetCode）
    链接：https://leetcode-cn.com/problems/last-stone-weight/
    

---
- 解题思路

    `题解1` 苯办法， 做递归。

- 题解1:

    执行用时：44 ms, 在所有 Python3 提交中击败了42.77%的用户
    内存消耗：14.8 MB, 在所有 Python3 提交中击败了38.92%的用户

```python
import heapq
class Solution:
    def top_two(self, stones):
        result = heapq.nlargest(2, stones)
        return max(result), min(result)

    def lastStoneWeight(self, stones: List[int]) -> int:
        if len(stones) == 0:
            return 0
        elif len(stones) == 1:
            return stones[0]
        elif len(stones) > 1:
            max, min = self.top_two(stones)
            if max == min:
                stones.remove(max)
                stones.remove(min)
                if len(stones) == 1:
                    return stones[0]
                elif len(stones) > 1:
                    return self.lastStoneWeight(stones)
                elif len(stones) == 0:
                    return 0
            else:
                stones.append(max-min)
                stones.remove(max)
                stones.remove(min)
                if len(stones) == 1:
                    return stones[0]
                return self.lastStoneWeight(stones)
```

