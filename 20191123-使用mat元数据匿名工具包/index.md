# 使用MAT元数据匿名工具包


- MAT：Metadata Anonymisation Toolkit
![](http://qiniu.aimiter.com/myblog/2019-11-27%2023-24-12.png)
### 什么是元数据Metadata?
元数据是描述其它数据的数据（data about other data）， 比如一张图片是图像数据，那么图片的*拍摄时间*，*拍摄地点*等就是它的元数据。
![](http://qiniu.aimiter.com/myblog/metadata2.jpg)

### 为什么要清除元数据？
因为元数据包含时间，地点等个人敏感隐私数据。网上不乏有黑客通过社交照片的元数据信息追踪到用户地址，从而进行骚扰勒索等危险行动。包括此文中的博客, 上传图床前已经过脱敏处理.
## 安装
```bash 
# 安装
apt install  mat 
#  启动图形界面
mat-gui
# 查看帮助信息
mat -h
usage: mat [-h] [-a] [-b] [-L] [-c] [-d] [-l] [-v] [files [files ...]]

Metadata anonymisation toolkit

positional arguments:
  files

optional arguments:
  -h, --help            show this help message and exit

Options:
  -a, --add2archive     add to output archive non-supported filetypes (Off by
                        default)
  -b, --backup, -b      keep a backup copy
  -L, --low-pdf-quality
                        produces a lighter, but lower quality PDF

Information:
  -c, --check           check if a file is free of harmful metadatas
  -d, --display         list all the harmful metadata of a file without
                        removing them
  -l, --list            list all supported fileformats
  -v, --version         show program's version number and exit

```
## 使用
```bash
#  参数 -c  检查是否含有元数据
mat -c metadata.jpg

[+] metadata.jpg is not clean

# 参数 -d 显示元数据
mat -d metadata.jpg

[+] File metadata.jpg :
Harmful metadata found:
	Orientation: Horizontal (normal)
	XMP Toolkit: XMP Core 4.4.0-Exiv2
	Exif Image Width: 1147
	Exif Image Height: 859
	Exif Byte Order: Little-endian (Intel, II)
	Software: Shotwell 0.28.4

# 删除元数据
mat metadata.jpg

[*] Cleaning metadata.jpg
[+] metadata.jpg cleaned!

#  再次检查
mat -c metadata.jpg

[+] metadata.jpg is clean

```

### 图形界面
添加， 清除两步操作很简单
![](http://qiniu.aimiter.com/myblog/2019-11-27%2023-22-06.png)

### 支持格式：
    Portable Network Graphics (.png)
    JPEG (.jpg, .jpeg, …)
    TIFF (.tif, tiff, …)
    Open Documents (.odt, .odx, .ods, …)
    Office OpenXml (.docx, .pptx, .xlsx, …)
    Portable Document Fileformat (.pdf)
    Tape ARchives (.tar, .tar.bz2, …)
    MPEG AUdio (.mp3, .mp2, .mp1, …)
    Ogg Vorbis (.ogg, …)
    Free Lossless Audio Codec (.flac)
    Torrent (.torrent)


## 备注
  1. 依赖Python2解释器
  2. 谨慎使用，不一定能清除所有的元数据， 尤其是深度自定义，水印或隐写数据。
