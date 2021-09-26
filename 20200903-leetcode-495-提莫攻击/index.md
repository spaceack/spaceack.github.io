# leetcode-495-提莫攻击


## 题目描述

在《英雄联盟》的世界中，有一个叫 “提莫” 的英雄，他的攻击可以让敌方英雄艾希（编者注：寒冰射手）进入中毒状态。现在，给出提莫对艾希的攻击时间序列和提莫攻击的中毒持续时间，你需要输出艾希的中毒状态总时长。

你可以认为提莫在给定的时间点进行攻击，并立即使艾希处于中毒状态。

提示：

你可以假定时间序列数组的总长度不超过 10000。
你可以假定提莫攻击时间序列中的数字和提莫攻击的中毒持续时间都是非负整数，并且不超过 10,000,000。

## 示例
- 示例 1:
    ```
    输入: [1,4], 2
    输出: 4
    原因: 第 1 秒初，提莫开始对艾希进行攻击并使其立即中毒。中毒状态会维持 2 秒钟，直到第 2 秒末结束。
    第 4 秒初，提莫再次攻击艾希，使得艾希获得另外 2 秒中毒时间。
    所以最终输出 4 秒。
    ```
- 示例 2:
    ```
    输入: [1,2], 2
    输出: 3
    原因: 第 1 秒初，提莫开始对艾希进行攻击并使其立即中毒。中毒状态会维持 2 秒钟，直到第 2 秒末结束。
    但是第 2 秒初，提莫再次攻击了已经处于中毒状态的艾希。
    由于中毒状态不可叠加，提莫在第 2 秒初的这次攻击会在第 3 秒末结束。
    所以最终输出 3 。
    ```

    来源：力扣（LeetCode）
    链接：https://leetcode-cn.com/problems/robot-return-to-origin
    

---
- 解题思路

    首先构造一个尽可能包含全部情况示例观察， 比如 `[2,5,9,10,11,15,20], 3`
    
     然后画个数轴，在上面模拟下，观测规律即可解出。


- 题解1:

    执行用时：328 ms, 在所有 Python3 提交中击败了64.05%的用户

    内存消耗：15 MB, 在所有 Python3 提交中击败了39.77%的用户

    ```python
    from typing import List
    class Solution:
        def findPoisonedDuration(self, timeSeries: List[int], duration: int) -> int:
            # 要对 timeSeries 为空的情况作判断， 因为这个没有一次通过，遗憾～
            if not timeSeries: 
                return 0
            sum_time = duration
            len_Serie = len(timeSeries)
            # 一个经常使用的小技巧： 遍历从 1 开始， 下标不用 +1 ， 避免数组越界
            for t in range(1, len_Serie):
                time_span = timeSeries[t] - timeSeries[t-1]
                # 最简单的情况， 两次攻击间隔大于 中毒时间， 直接累加。
                if time_span >= duration:
                    sum_time = sum_time + duration
                # 当攻击间隔小于中毒时间，要认真思考。
                else:
                    last_time_span = duration - time_span
                    sum_time = sum_time + duration - last_time_span
            return sum_time
    ```

