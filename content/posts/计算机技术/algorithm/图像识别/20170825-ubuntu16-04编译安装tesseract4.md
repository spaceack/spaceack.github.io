---
title: Ubuntu16.04 下编译安装tesseract 4.00.00alpha 及测试
date: 2017-08-25 12:00:00
update: 2017-08-25 12:00:00
author: Spaceack
categories: ["Linux"]
tags: 
  - 图像识别
  - tesseract
---

1. 3.05.01 及 以后的版本没有Linux的二进制包,需要编译安装.
```bash
# 安装相关组件
sudo apt-get install g++ # or clang++ (presumably)
sudo apt-get install autoconf automake libtool
sudo apt-get install autoconf-archive
sudo apt-get install pkg-config
sudo apt-get install libpng12-dev
sudo apt-get install libjpeg8-dev
sudo apt-get install libtiff5-dev
sudo apt-get install zlib1g-dev
sudo apt-get install libicu-dev
sudo apt-get install libpango1.0-dev
sudo apt-get install libcairo2-dev
```
2. 依赖图像库[Leptonica](http://www.leptonica.org/download.html),在编译tesseract前先编译Leptonica, 版本对应关系见[Compiling#linux](https://github.com/tesseract-ocr/tesseract/wiki/Compiling#linux),3.05对应[leptonica-1.74.tar.gz](http://www.leptonica.org/source/leptonica-1.74.tar.gz)
```bash
wget http://www.leptonica.org/source/leptonica-1.74.tar.gz
tar -xvf leptonica-1.74.tar.gz
cd leptonica-1.74
./configure 
make
make install
```

3. 编译安装tesseract 4.00.00alpha
```bash
git clone https://github.com/tesseract-ocr/tesseract.git
cd tesseract
./autogen.sh
./configure 
make
make install
```
---
- [wiki](https://github.com/tesseract-ocr/tesseract/wiki)
### python3 调用
```bash
sudo pip3 install pytesseract
```
```python
import pytesseract
vcode = pytesseract.image_to_string(im, lang='eng', config='-psm 12 --tessdata-dir /tessdata/')
```
