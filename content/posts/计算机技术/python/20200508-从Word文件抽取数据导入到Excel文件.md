---
title: 从Word文件抽取数据导入到Excel文件
author: Spaceack
date: 2020-05-08 21:00:00
update:  2020-05-08 21:00:00
categories: ["Python", "数据处理"]
tags: 
  - docx
  - xlwl
---

```bash
pip3 install python-docx;
pip3 install xlwl;

```
### 从docx抽取数据
```python
import docx
def get_docx():
  from docx import Document
  path = "info.docx"
  document = Document(path)
  Lines = []
  for paragraph in document.paragraphs:
      Lines.append(paragraph.text)
  return Lines
```
### 写入xls文件
```python
import xlwt
workbook = xlwt.Workbook(encoding = 'utf-8')
worksheet = workbook.add_sheet('My Worksheet')
# 0行 0列 写入 something
worksheet.write(0, 0, label='something')
workbook.save('result.xls')
```

### 结合使用
```python
import docx, xlwt
from docx import Document

def get_docx():
  path = "info.docx"
  document = Document(path)
  Lines = []
  for paragraph in document.paragraphs:
      Lines.append(paragraph.text)
  return Lines

def main():
    workbook = xlwt.Workbook(encoding = 'utf-8')
    worksheet = workbook.add_sheet('My Worksheet')

    contents = get_docx()
    for index, content in enumerate(contents):
        print(content, index)
        worksheet.write(index, 1, label=content)
        workbook.save('result.xls')

if __name__ == '__main__':
    main()
```
