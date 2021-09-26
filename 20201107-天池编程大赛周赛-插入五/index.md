# 天池编程大赛周赛-插入五


## 题目描述

给定一个数字，在数字的任意位置插入一个5，使得插入后的这个数字最大

$$|a| \leq 10^6$$

## 示例

- 示例 1:
    ```
    输入:  a = 234
    输出: 5234    
    ```
    来源：九章算法
    链接：https://tianchi.aliyun.com/oj/141758389886413149/160295184768372892
    

---

- 解题思路
    
很简单的模拟题， 但是还是要注意审题，一看到带有绝对值的取值范围，就要立刻想到负数情况。
又是第二次才AC， 有点儿遗憾。

- 错误的题解示范:
   
    没有考虑到负数的情况

    ```python
    class Solution:
    """
    @param a: A number
    @return: Returns the maximum number after insertion
    """
    def InsertFive(self, a):
        # write your code here
        result = []
        used = False
        for s in str(a):
            if 5 >= int(s) and not used:
                result.append('5')
                result.append(s)
                used = True
            else:
                result.append(s)
        if not used:
            result.append('5')
        return int("".join(result))
    ```

- 题解1:
  
  正确的示范

  ```python
  class Solution:
      """
      @param a: A number
      @return: Returns the maximum number after insertion
      """
      def InsertFive(self, a):
          # write your code here
          result = []
          # 插入过5的标记
          used = False
          # 判断正负两种情况
          # 正数情况
          if a >=0:
            for s in str(a):
                if 5 >= int(s) and not used:
                    result.append('5')
                    result.append(s)
                    used = True

                else:
                    result.append(s)
            if not used:
                result.append('5')
            return int("".join(result))
          # 负数情况
          else:
              for s in str(abs(a)):
                  if 5 < int(s) and not used:
                      result.append('5')
                      result.append(s)
                      used = True

                  else:
                      result.append(s)
              if not used:
                  result.append('5')
              return -int("".join(result))
  ```
