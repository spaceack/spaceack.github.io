---
title: 蓝桥楼赛第23期-新冠疫情数据统计 题解
author: Spaceack
date: 2020-09-20 21:00:00
update: 2020-09-20 21:00:00
categories: ["Python","算法"]
tags: 
  - python
  - country-converter
  - 中等
  - 题解
  - 比赛
---

## 题目描述

2020 年，新冠疫情肆掠全球。约翰·霍普金斯大学 跟踪了全球病例数据，包括总病例数、COVID-19 传播速度以及全球爆发情况。我们拿到了截止于某日的疫情数据，希望通过 Python 统计出我们需要的疫情指标。

## 目标

补充 count(data) 函数中的 TODO 部分，使其得到我们需要的结果：

- 整理指定 data 数据文件，以 JSON 数据返回世界各大洲的的汇总数据。
- 数据集中仅存在国家和地区名称，不存在大洲数据，需要自行解决。表格中的每个国家/地区都需要划分到实际所在大洲。
- 需要删除明显统计错误的数据（即：确诊人数、死亡人数、康复人数、现有人数不匹配），缺失人数统计数据使用 0 填充。其余情况无需处理。

```python
def count(data):
    """TODO
    """
    results = None
    return results
```

## 要求

- 题目需使用 Python 3.6 完成，可以使用标准库和第三方库。如果你的函数使用了第三方库，提交检测前，务必在线上环境中安装相应库。
- 使用第三方库时，必须使用 python3.6 -m pip install <package_name> 命令安装，保证相应库安装在 Python 3.6 环境中。
- 函数传入 data 为字符串类型，为数据文件的相对路径。
- 函数返回 JSON 数据（字符串类型），示例如上，无顺序要求。
- 需要将函数 count(data) 保存到 covid.py 文件中，并将该文件放置在 /home/shiyanlou/Code 路径下方。
- covid.py 文件中仅保留函数，不要添加测试或执行代码，避免检测时出错。
- 线上环境调试代码时，请使用 python3.6 covid.py 命令调用 Python 3.6。

## 提示

- country-converter 库提供了转换大洲数据的方法，你可以通过官方提供的 示例学习。线上环境中安装 country-converter 的命令为：python3.6 -m pip install setuptools && python3.6 -m pip install country-converter

```python
def clean_up(folder):
    """TODO
    """
    file_list = {"folder_name":file_nums}
    return file_list
```

## 示例

首先，打开终端，使用以下命令将数据文件下载至环境中：

```bash
cd /home/shiyanlou/Code
wget https://labfile.oss.aliyuncs.com/courses/2799/cases_country.csv
```
[cases_country.csv](cases_country.csv)

部分数据截图如下，其中 ISO3 为国家/地区标准代码：

![1.png](1.png)

count(data) 函数最终返回数据格式示例如下（数据非真实情况）：


```bash
(results = {
  "Confirmed": {
    "Africa": 1203024,
    "Asia": 6420215,
    "Oceania": 25346,
    "Europe": 3311213,
    "America": 1023402,
    "Others": 13443,
    "Total": 15440234
  },
  "Deaths": {
    "Africa": 22222,
    "Asia": 133126,
    "Oceania": 556,
    "Europe": 111431,
    "America": 51155,
    "Others": 502,
    "Total": 616513
  },
  "Recovered": {
    "Africa": 130522,
    "Asia": 5163035,
    "Oceania": 21212,
    "Europe": 1112545,
    "America": 214106,
    "Others": 1424,
    "Total": 13131033
  },
  "Active": {
    "Africa": 244262,
    "Asia": 1124052,
    "Oceania": 4252,
    "Europe": 1201515,
    "America": 121345,
    "Others": 3455,
    "Total": 3612602
  }
})
```

返回数据中，Confirmed，Deaths，Recovered，Active 分别表示：确诊人数、死亡人数、康复人数、现有人数。而 Africa，Asia，Oceania，Europe，America，Others 分别表示：非洲、亚洲、大洋洲、欧洲、美洲（北美洲和南美洲）和其他的相应人数，Others 其他为非国家/地区的数据项。Total 表示数据总和。所有数值数据为 Int 类型。


    来源：蓝桥（实验楼）
    链接：https://www.lanqiao.cn/challenges/50212/
    

---

- 解题思路
    
    直接使用 `country-converter` 库可能会超时， 而且线上环境容易忘记安装。 这里首先用 `country-converter`库构造一个ISO3码对应大洲的字典。把字典直接加入到代码。

    ```python
    ISO3_converter = {}
    import country_converter as coco
    #...此处应是一个for循环...
    ISO3_converter[iso3_code] = coco.convert(names=[[iso3_code]], to='converter')
    # ...
    ```

    最后根据题意， 以json格式输出。

- 题解1:

    ```python
    import csv
    import json

    ISO3_converter = {'AFG': 'Asia', 'ALB': 'Europe', 'DZA': 'Africa', 'AND': 'Europe', 'AGO': 'Africa', 'ATG': 'America', 'ARG': 'America', 'ARM': 'Asia', 'AUS': 'Oceania', 'AUT': 'Europe', 'AZE': 'Asia', 'BHS': 'America', 'BHR': 'Asia', 'BGD': 'Asia', 'BRB': 'America', 'BLR': 'Europe', 'BEL': 'Europe', 'BLZ': 'America', 'BEN': 'Africa', 'BTN': 'Asia', 'BOL': 'America', 'BIH': 'Europe', 'BWA': 'Africa', 'BRA': 'America', 'BRN': 'Asia', 'BGR': 'Europe', 'BFA': 'Africa', 'MMR': 'Asia', 'BDI': 'Africa', 'CPV': 'Africa', 'KHM': 'Asia', 'CMR': 'Africa', 'CAN': 'America', 'CAF': 'Africa', 'TCD': 'Africa', 'CHL': 'America', 'CHN': 'Asia', 'COL': 'America', 'COM': 'Africa', 'COG': 'Africa', 'COD': 'Africa', 'CRI': 'America', 'CIV': 'Africa', 'HRV': 'Europe', 'CUB': 'America', 'CYP': 'Asia', 'CZE': 'Europe', 'DNK': 'Europe', 'DJI': 'Africa', 'DMA': 'America', 'DOM': 'America', 'ECU': 'America', 'EGY': 'Africa', 'SLV': 'America', 'GNQ': 'Africa', 'ERI': 'Africa', 'EST': 'Europe', 'SWZ': 'Africa', 'ETH': 'Africa', 'FJI': 'Oceania', 'FIN': 'Europe', 'FRA': 'Europe', 'GAB': 'Africa', 'GMB': 'Africa', 'GEO': 'Asia', 'DEU': 'Europe', 'GHA': 'Africa', 'GRC': 'Europe', 'GRD': 'America', 'GTM': 'America', 'GIN': 'Africa', 'GNB': 'Africa', 'GUY': 'America', 'HTI': 'America', 'VAT': 'Europe', 'HND': 'America', 'HUN': 'Europe', 'ISL': 'Europe', 'IND': 'Asia', 'IDN': 'Asia', 'IRN': 'Asia', 'IRQ': 'Asia', 'IRL': 'Europe', 'ISR': 'Asia', 'ITA': 'Europe', 'JAM': 'America', 'JPN': 'Asia', 'JOR': 'Asia', 'KAZ': 'Asia', 'KEN': 'Africa', 'KOR': 'Asia', 'KWT': 'Asia', 'KGZ': 'Asia', 'LAO': 'Asia', 'LVA': 'Europe', 'LBN': 'Asia', 'LSO': 'Africa', 'LBR': 'Africa', 'LBY': 'Africa', 'LIE': 'Europe', 'LTU': 'Europe', 'LUX': 'Europe', 'MDG': 'Africa', 'MWI': 'Africa', 'MYS': 'Asia', 'MDV': 'Asia', 'MLI': 'Africa', 'MLT': 'Europe', 'MRT': 'Africa', 'MUS': 'Africa', 'MEX': 'America', 'MDA': 'Europe', 'MCO': 'Europe', 'MNG': 'Asia', 'MNE': 'Europe', 'MAR': 'Africa', 'MOZ': 'Africa', 'NAM': 'Africa', 'NPL': 'Asia', 'NLD': 'Europe', 'NZL': 'Oceania', 'NIC': 'America', 'NER': 'Africa', 'NGA': 'Africa', 'MKD': 'Europe', 'NOR': 'Europe', 'OMN': 'Asia', 'PAK': 'Asia', 'PAN': 'America', 'PNG': 'Oceania', 'PRY': 'America', 'PER': 'America', 'PHL': 'Asia', 'POL': 'Europe', 'PRT': 'Europe', 'QAT': 'Asia', 'ROU': 'Europe', 'RUS': 'Europe', 'RWA': 'Africa', 'KNA': 'America', 'LCA': 'America', 'VCT': 'America', 'SMR': 'Europe', 'STP': 'Africa', 'SAU': 'Asia', 'SEN': 'Africa', 'SRB': 'Europe', 'SYC': 'Africa', 'SLE': 'Africa', 'SGP': 'Asia', 'SVK': 'Europe', 'SVN': 'Europe', 'SOM': 'Africa', 'ZAF': 'Africa', 'SSD': 'Africa', 'ESP': 'Europe', 'LKA': 'Asia', 'SDN': 'Africa', 'SUR': 'America', 'SWE': 'Europe', 'CHE': 'Europe', 'SYR': 'Asia', 'TWN': 'Asia', 'TJK': 'Asia', 'TZA': 'Africa', 'THA': 'Asia', 'TLS': 'Asia', 'TGO': 'Africa', 'TTO': 'America', 'TUN': 'Africa', 'TUR': 'Asia', 'USA': 'America', 'UGA': 'Africa', 'UKR': 'Europe', 'ARE': 'Asia', 'GBR': 'Europe', 'URY': 'America', 'UZB': 'Asia', 'VEN': 'America', 'VNM': 'Asia', 'PSE': 'Asia', 'ESH': 'Africa', 'YEM': 'Asia', 'ZMB': 'Africa', 'ZWE': 'Africa'}

    def count(data):
        """TODO
        """
        Confirmed = {"Africa": 0, "Asia": 0, "Oceania": 0,"Europe": 0, "America": 0, "Others": 0, "Total": 0}
        Deaths = {"Africa": 0, "Asia": 0, "Oceania": 0,"Europe": 0, "America": 0, "Others": 0, "Total": 0}
        Recovered = {"Africa": 0, "Asia": 0, "Oceania": 0,"Europe": 0, "America": 0, "Others": 0, "Total": 0}
        Active = {"Africa": 0, "Asia": 0, "Oceania": 0,"Europe": 0, "America": 0, "Others": 0, "Total": 0}

        with open(data, 'r') as f:
            reader = csv.reader(f)
            for row in reader:
                if row and 'Lat' not in row:
                    code = row[13]
                    if code:
                        if row[13] == 'XKS':
                            continent = 'Europe'
                        else:
                            continent = ISO3_converter[code]
                    else:
                        continent = 'Others'
                    confirmed = row[4]
                    confirmed = int(float(confirmed)) if confirmed else 0

                    deaths = row[5]
                    deaths = int(float(deaths)) if deaths else 0

                    recovered = row[6]
                    recovered = int(float(recovered)) if recovered else 0

                    active = row[7]
                    active = int(float(active)) if active else 0

                    if confirmed != (deaths + recovered + active):
                        continue

                    Confirmed[continent] = Confirmed[continent] + confirmed
                    Deaths[continent] = Deaths[continent] + deaths
                    Recovered[continent] = Recovered[continent] + recovered
                    Active[continent] = Active[continent] + active

            Confirmed['Total'] = Confirmed['Africa'] + Confirmed['Asia'] + Confirmed['Oceania'] + Confirmed['Europe'] + Confirmed['America'] + Confirmed['Others']
            Deaths['Total'] = Deaths['Africa'] + Deaths['Asia'] +Deaths['Oceania'] +Deaths['Europe'] + Deaths['America'] + Deaths['Others']
            Recovered['Total'] = Recovered['Africa'] + Recovered['Asia'] + Recovered['Oceania'] + Recovered['Europe'] + Recovered['America'] + Recovered['Others']
            Active['Total'] = Active['Africa'] + Active['Asia'] + Active['Oceania'] + Active['Europe'] + Active['America'] + Active['Others']

        results = {
            "Confirmed": Confirmed,
            "Deaths": Deaths,
            "Recovered": Recovered,
            "Active": Active,
        }

        return json.dumps(results, indent=2)

    
    ```
