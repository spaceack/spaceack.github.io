# 《挑战程序设计(第二版)》（第二章）POJ No.2386


### POJ 2386
- 问题:
- 有一个大小为N×M的园子，雨后积起了水。八连通的积水被认为是连在一起的。求园子里一共有多少水洼？\
- 限制条件N,M<=100
- 输入
```
W........WW.
.WWW.....WWW
....WW...WW.
.........WW.
.........W..
..W......W..
.W.W.....WW.
W.W.W.....W.
.W.W......W.
..W.......W.
```
- 输出: 3
- 题解:
```
DFS次数为水洼的个数，
```
- POJ No.2386 python 实现:
```python
# POJ No.2386
class poj2386(object):
    def __init__(self, map):
        self.row_num = 10
        self.col_num = 12
        self.m = self.matrix(map.replace('\n',''))
        self.walked_set = set()
        self.dfs_num = 0

    def matrix(self, map):
        '''
        desc: 字符串转数组，方便遍历
        :return ['row0','row1'...]
        '''
        col_num = 0
        matrix = []
        for r in range(self.row_num):
            matrix.append(map[col_num: col_num + self.col_num])
            col_num = col_num + self.col_num
        return matrix

    def dfs(self, x, y, char):
        '''
        desc:用递归实现搜索范围内相同字符
        :param x: 
        :param y: 
        :param char: 
        :return: 
        '''
        if 0 > x or 0 > y or x >= self.row_num  or y >= self.col_num : # 越界检查
            return
        if tuple([x,y]) in self.walked_set: # 重复遍历检查
            return
        if char != self.m[x][y]: # 目标字符检查
            return

        self.walked_set.add(tuple([x, y]))

        self.dfs(x + 1, y, char)  # x
        self.dfs(x, y + 1, char)  # y
        self.dfs(x - 1, y, char)  # -x
        self.dfs(x, y - 1, char)  # -y
        self.dfs(x + 1, y + 1, char)  # Ⅰ
        self.dfs(x + 1, y - 1, char)  # Ⅱ
        self.dfs(x - 1, y - 1, char)  # Ⅲ
        self.dfs(x - 1, y + 1, char)  # Ⅳ
        return

    def walk(self):
        '''
        desc: 对矩阵遍历
        :return: 
        '''
        for x in range(self.row_num-1):
            for y in range(self.col_num-1):
                if tuple([x, y]) in self.walked_set:
                    continue
                if self.m[x][y] == '.':
                    continue
                if self.m[x][y] == 'W':
                    self.dfs(x, y, 'W')
                    self.dfs_num +=1
        return self.dfs_num
if __name__ == '__main__':
    map = '''
W........WW.
.WWW.....WWW
....WW...WW.
.........WW.
.........W..
..W......W..
.W.W.....WW.
W.W.W.....W.
.W.W......W.
..W.......W.
'''
    c = poj2386(map)
    dfs_num = c.walk()
    print(dfs_num)
```

