# Python基础操作

## 字典
### 合并两个字典 merge two dictionaries
```python
dict1 = {'a': 1, 'b': 2}
dict2 = {'c': 3, 'd': 4}
dict3 = {**dict1, **dict2} # python3.5+ 写法
dict3 = dict(dict1, **dict2) # python3.5- 写法
print(dict3)
```

### 字典按值排序 sort a Python dict by value
```python
xs = {'a': 4, 'b': 3, 'c': 2, 'd': 1}
for k, v in sorted(xs.items(), key=lambda x: x[1]):
    print(k, v)
```

```python
import operator
xs = {'a': 4, 'b': 3, 'c': 2, 'd': 1}
for k, v in sorted(xs.items(), key=operator.itemgetter(1)):
    print(k, v)
```
