# ubuntu18.04编译安装python3.8

## 下载
```bash
# 下载源码包至`opt`目录
wget -c -P /opt https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tar.xz
# 解压解包
tar - xvf Python-3.8.0.tar.xz 
cd Python-3.8.0
```
## 编译安装
```bash
# 更新系统
apt-get update
apt-get upgrade
apt-get dist-upgrade
# 安装依赖库
apt-get install libbz2-dev libncurses5-dev libgdbm-dev libgdbm-compat-dev liblzma-dev libsqlite3-dev libssl-dev openssl tk-dev uuid-dev libreadline-dev  python-dev

./configure --enable-optimizations --enable-shared
make -j8
make altinstall
```
## 运行测试
```bash
python3.8
# python3.8: error while loading shared libraries: libpython3.8.so.1.0: cannot open shared object file: No such file or directory
```
配置动态链接库路径
```bash
vim ~/.profile
# 追加
export LD_LIBRARY_PATH="/usr/local/lib"
alias python="/usr/local/bin/python3.8"
alias python3.8="/usr/local/bin/python3.8"
# 更新
source ~/.profile
```
再次测试成功！

