# 蓝桥楼赛第23期-工作文件整理归类 题解


## 题目描述

实小楼同学平常的工作比较繁杂，经常需要处理各类文档，几天时间桌面上就累积了一堆不同类型和名称的文档，显得十分杂乱。实小楼想通过 Python 编写一个脚本，能够自动归类整理不同类型的文档。

## 目标

补充 clean_up(folder) 函数中的 TODO 部分，使其实现我们需要的功能：

- 归类整理指定 folder 文件夹中的不同类型文档，如上方示意图所示。
- 如果存在多个不同类型，但名称相同的文件，则归类为同一文件夹中， 并将此文件夹命名为与文件一致的名称。
- 其余名称不同，类型相同的文件，则按照文件类型归类为同一文件夹中，并将此文件夹命名为文档类型名称。
- 如果文件无类型后缀，则统一存放至名称为 others 的文件夹中。
- 整理后的文件和文件夹均存放在 folder 文件夹中，并移除原文档。
- 函数最终返回字典类型的 file_list，包含整理后的文件夹名称和文件夹中包含的文件数量。

## 要求

- 题目需使用 Python 3.6 完成，可以使用标准库，不能使用第三方库。
- 函数传入 folder 为字符串类型，是脚本文件和所需整理目录的相对路径。
- 函数返回字典，且应按不同 folder_name 中 file_nums 的数字降序排列，次数相等无先后顺序。
- 需要将函数 clean_up(folder) 保存到 clean_up_files.py 文件中，并将该文件放置在 /home/shiyanlou/Code 路径下方。
- clean_up_files.py 文件中仅保留函数，不要添加测试或执行代码，避免检测时出错。
- 线上环境调试代码时，请使用 python3.6 clean_up_files.py 命令调用 Python 3.6。

## 提示

- 文件名中可以存在 . 符号，例如 test.a.csv，这是名为 test.a 的 .csv 文件。

```python
def clean_up(folder):
    """TODO
    """
    file_list = {"folder_name":file_nums}
    return file_list
```

## 示例

首先，打开终端，使用以下命令生成测试文档：

```bash
mkdir test_files && cd test_files
touch project_{a..e}.py && touch project_{a..c}.psd && touch doc_{1..5}.docx && touch data_{5..9}.csv && touch file_{1..3}
```

根据上述生成的测试文档，最终返回结果应该为：

```bash
file_list = {
  "csv": 5,
  "docx": 5,
  "others": 3,
  "py": 2,
  "project_b": 2,
  "project_c": 2,
  "project_a": 2
}
```

整理后的目录结构为：

```
.
├── csv
│   ├── data_5.csv
│   ├── data_6.csv
│   ├── data_7.csv
│   ├── data_8.csv
│   └── data_9.csv
├── docx
│   ├── doc_1.docx
│   ├── doc_2.docx
│   ├── doc_3.docx
│   ├── doc_4.docx
│   └── doc_5.docx
├── others
│   ├── file_1
│   ├── file_2
│   └── file_3
├── project_a
│   ├── project_a.psd
│   └── project_a.py
├── project_b
│   ├── project_b.psd
│   └── project_b.py
├── project_c
│   ├── project_c.psd
│   └── project_c.py
└── py
    ├── project_d.py
    └── project_e.py
```

注意：最终实现效果以完全满足要求为准，而不是仅满足生成的示例测试文档。


- 示例 2:
    ```
    text = "@实验楼@shiyanlou 我在 @ 楼赛中中奖啦"; 
    usernames = ['实验楼', 'shiyanlou']

    ```

    来源：蓝桥（实验楼）
    链接：https://www.lanqiao.cn/challenges/50212/
    

---

- 解题思路
    
    考察os, shutil标准库的使用

- 题解1:

    ```python
    import os
    import shutil
    from typing import Dict, List


    def clean_up(folder):
        """
        """
        # 相对路径
        relative_path = folder+os.sep

        # 对文件名相同的文件过滤，按照"文件名": [完整文件名] 的键值存入字典， 最后对列表文件数大于1的文件移入新目录。
        for root, dirs, files in os.walk(folder):
            name_to_files = {}   # type: Dict[str: List[str]] # {"文件名1": ["完整文件名", "完整文件名2" ...]， ...}
            for f in files:
                if '.' in f:
                    # 分离文件名，扩展名，这里获取文件名
                    fn = os.path.splitext(f)[0]
                    # 存入字典
                    name_to_files[fn] = name_to_files[fn] + [f, ] if fn in name_to_files else [f, ]
            # 遍历字典，创建目录，移动文件
            for key in name_to_files:
                if len(name_to_files[key]) > 1:
                    os.mkdir(relative_path+key)
                    for file_name in name_to_files[key]:
                        # 移动文件
                        shutil.move(relative_path+file_name, relative_path+key)

        # 对扩展名相同的文件过滤，按照"文件名": [完整文件名] 的键值存入字典， 最后对列表文件数大于1的文件移入新目录。
        for root, dirs, files in os.walk(folder):
            extension_to_files = {}  # type: Dict[str: List[str]] # {"扩展名": ["完整文件名", "完整文件名2" ...]， ...}
            for f in files:
                if '.' in f:
                    # 分离文件名，扩展名，这里获取扩展名， 取出来的带有'.' ，这里用[1:]过滤
                    fn = os.path.splitext(f)[1][1:]
                    extension_to_files[fn] = extension_to_files[fn] + [f, ] if fn in extension_to_files else [f, ]
            # 遍历字典，创建目录，移动文件
            for key in extension_to_files:
                if len(extension_to_files[key]) > 1:
                    os.mkdir(relative_path+key)
                    for extension_name in extension_to_files[key]:
                        shutil.move(relative_path+extension_name, relative_path+key)

        # 剩下对没有扩展名的文件处理
        for root, dirs, files in os.walk(folder):
            if files:
                if os.path.exists(relative_path + 'others'):
                    pass
                else:
                    os.mkdir(relative_path + 'others')
                for f in files:
                    shutil.move(relative_path + f, relative_path+'others')
                break

        file_list = {}  # type: Dict[str: int]
        folder_dirs = os.listdir(folder)
        # 存入目录名和对应的目录文件数
        for fd in folder_dirs:
            file_list[fd] = len([lists for lists in os.listdir(relative_path + fd)
                                if os.path.isfile(os.path.join(relative_path + fd, lists))])
        # 按值排序
        file_list = sorted(file_list.items(), key=lambda x: x[1], reverse=True)
        return dict(file_list)

    
    ```

