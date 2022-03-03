# ubuntu16.04(18.04)下编译安装 postgresql v10.3


### 编译安装

```bash
# downloads postgresql-10.3.tar.gz
wget https://ftp.postgresql.org/pub/source/v10.3/postgresql-10.3.tar.gz
wget https://ftp.postgresql.org/pub/source/v10.3/postgresql-10.3.tar.gz.md5
wget https://ftp.postgresql.org/pub/source/v10.3/postgresql-10.3.tar.gz.sha256

# MD5 check 检查数据是否被篡改
cat postgresql-10.3.tar.gz.md5
md5sum  postgresql-10.3.tar.gz

# extract
tar -xvf postgresql-10.3.tar.gz

cd postgresql-10.3/

# 安装文档介绍的很详细
cat INSTALL

./configure

# 提示缺少readline library， 尝试安装libreadline5-dev 提示已被libreadline-gplv2-dev 替代， 安装之
checking for library containing readline... no
configure: error: readline library not found

sudo apt install libreadline5-dev

However the following packages replace it:
  libreadline-gplv2-dev:i386 libreadline-gplv2-dev

sudo apt install libreadline-gplv2-dev


checking for inflate in -lz... no
configure: error: zlib library not found
# 又提示缺失zlib library， 安装之
sudo apt install zlib1g-dev 

# 后边编译安装就顺利了：）
make
sudo make install
sudo adduser postgres
sudo mkdir /usr/local/pgsql/data
sudo chown postgres /usr/local/pgsql/data
su - postgres
# 初始化数据库
/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
or
initdb -D  /usr/local/pgsql/data

# start the database server
/usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >logfile 2>&1 &
or
pg_ctl -D /usr/local/pgsql/data -l logfile start
# 测试成功
/usr/local/pgsql/bin/createdb test
/usr/local/pgsql/bin/psql test

# set postgresql env to .bashrc
export PGHOME=/usr/local/pgsql
export PGDATA=/usr/local/pgsql/data
export PATH=$PGHOME/bin:$PATH:$HOME/bin
```
### 创建用户
```sql
# CREATEDB是权限，还有其它权限，比如SUPERUSER、CREATEUSER等。一般情况下CREATEDB是最安全的做法。因为这个test用户的权限被限制在很小范围。
CREATE USER username CREATEDB; 
# 修改用户密码
ALTER USER username PASSWORD 'password';
```



### 设置远程访问权限

```bash
# edit postgresql.conf '*' replace 'localhost'
vim /usr/local/pgsql/data/postgresql.conf
#listen_addresses = 'localhost'
listen_addresses = '*'

# edit pg_hba.conf
vim /usr/local/pgsql/data/pg_hba.conf

# IPv4 local connections: add username authentication
host    all             all             127.0.0.1/32            trust
host    all             username        0.0.0.0/0               md5

# 重启使之生效
su - postgres
pg_ctl restart
```
