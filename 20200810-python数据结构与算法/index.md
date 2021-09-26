# Python数据结构与算法


## 线性表
python的`list`是可变线性表。
1. len()是 O（1）操作
2. 元素访问和赋值，尾端加入和尾端删除（包括尾端切片删除）都是O（1）操作。
3. 一般位置的元素加入，切片替换，切片删除，表拼接（extend）都是O（n）操作。
4. pop操作默认为删除表尾元素并将其返回O（1），指定非尾端位置为O（n）时间复杂度。

5. `lst.clear()`清除表lst所有元素O（1）操作。两种实现方式：  
   a. 元素记数值（表长度）设置为0。实现简单，操作效率高，但不能真正释放占用的存储。若表很长，执行操作后表内没有元素，但仍会占用原有大块内存。
   
   b. 另分配一个空表用的存储区，原存储区直接丢弃。若表又一次增大，会频繁更换存储区。

6. `lst.reverse()`修改表lst自身，元素顺序倒置O（n）
    ```python
    def reverse(self):
        elems = self.elements
        i, j = 0, len(elems) - 1
        while i < j：
            elems[i], elems[j] = elems[j], elems[i]
            i, j = i+1, j-1
    ```
7. `sort`排序， 最好的排序算法的平均和最坏情况时间复杂度都是O（nlogn）

- 最重要特点（优势）是O（1）时间定位元素访问，许多简单操作效率较高。
- 缺陷是加入/删除操作的效率问题，插入删除可能要移动很多元素，操作代价高。
- 只有特殊的尾端插入/删除操作具有O(1)时间复杂度。但插入操作受元素存储区固定大小限制。通过加倍存储区扩充策略（初始分配为空，第一次增长到4个单元，增长率为1.125倍。1.保证O（1）的append,2.防止过多的空闲单元。），尾端插入可达到O（1）。

```

## 链接表
### 条件
1. 能够找到表中的首元素。
2. 从任一元素出发，可以找到下一个元素。

### 单链表
- 定义
```python
class LNode:
    def __init__(self, elem, next_= None):
        self.elem = elem
        self.next = next_
```

## 栈

栈（stack）又名堆栈，是一种运算受限的线性表。其限制是仅允许在表的一端进行插入和删除运算。

允许进行插入和删除操作的一段称为栈顶（top）,另一端称为栈底（bottom）

栈底固定，栈顶浮动。

栈中元素个数为零时称为**空栈**。插入一般称为**进栈（PUSH）**， 删除则称为**退栈（POP）**。

后进先出（LIFO，Last In First Out）, 后进先出表。

进栈和退栈时间复杂度都为O（1）

```python
class Stack(Object):
    def __init__(self, limit=10):
    """ 创建空栈
    """
        self.stack = [] # 存放元素
        self.limit = limit # 栈容量限制
    
    def push(self, data):
    """ 将元素data加入栈，也常称为压入或推入
    """
        # 判断栈是否溢出
        if len(self.stack) >= self.limit:
            raise IndexError('超出栈的最大容量！')
        self.stack.append(data)

    def pop(self):
    """ 删除栈里最后压入的元素并将其返回，常称为弹出。
    """
        if self.stack:
            return self.stack.pop()
        else:
            rasie IndexError('pop from an empty stack')

    def top(self):
    """ 取得栈里最后压入的（最上面的）元素，不删除。
    """
        if self.stack:
            return self.stack[-1]

    def is_empty(self):
    """ 判断栈是否为空， 空时返回True, 否则返回 False
    """
        return not bool(self.stack)

    def size(self):
    """ 取得栈中的元素个数
    """
        return len(self.stack) 

```

### 应用：括号匹配

```python
"""
使用一个堆栈检查括号字符串是否平衡

有效括号字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。
举例：

((())): True

((()): False

(())): False
"""
import stack
st = stack.Stack()

input_str = "(()"

for s in input_str:
	if st.top() == s:
		st.pop()
	else:
		st.push(s)

result = True if st.is_empty() else False
print(result)
```
