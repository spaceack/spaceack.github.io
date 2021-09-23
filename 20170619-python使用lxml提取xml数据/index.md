# 使用lxml提取HTML/XML 数据


# demo

```python
#更新： 新版本引入etree模块方式

from lxml import html
etree = html.etree
tree = etree.HTML("")
```

```python
html = '''
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title class="sub_title">Title</title>
</head>
<body>
<div class="none">
<a href ="#">Spaceack's code</a>
</div>
<div class="link">
<a href ="http://spaceack.com">Spaceack's blog</a>
</div>
</body>
</html>
'''

from lxml import etree
tree = etree.HTML(html)
# 获取class为link的a标签的元素内容
a_content = tree.xpath('.//div[@class="link"]/a/text()')
print(a_content)
# ["Spaceack's blog"]

# 使用attrib获取标签的属性值
href_element = tree.xpath('.//div[@class="link"]/a')
print(href_element)
# [<Element a at 0x7ff3571a4d80>]
href = href_element[0].attrib.get('href')
print(href)
# http://spaceack.com

```


### 获取标签元素内容为空的两种不同效果:
- demo
```python
from lxml import etree
# 可见第二个标签 td 元素内容为空
tree = etree.HTML("<th>水果</th><td>苹果</td><th>价格</th><td></td>")
key = tree.xpath("//th/text()")
value = tree.xpath("//td/text()")
print(key, value)
'''
['水果', '价格'] ['苹果']
'''
```
如果想让价格使用`None`或空字符串`''`来占位，可以这样做:

```python
key = tree.xpath("//th")
value = tree.xpath("//td")

key = [item.text for item in key] 
value = [item.text for item in value]
print(key, value)
'''
['水果', '价格'] ['苹果', None]
'''
key = [""  if   item.text==None else item.text for item in key]
value = [""  if  item.text==None else item.text for item in value]
print(key, value)
'''
key ['水果'， '价格']
['水果', '价格'] ['苹果', '']
'''

```
