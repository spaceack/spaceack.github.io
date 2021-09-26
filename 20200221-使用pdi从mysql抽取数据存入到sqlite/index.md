# 使用PDI从Mysql抽取数据存入到Sqlite

### 1.建立Mysql连接
![image.png](1-0.png)
### 2.建立Sqlite连接
自定义连接URL：jdbc:sqlite:/data/testdb.sqlite3
自定义驱动类型 `org.sqlite.JDBC`
![image.png](1-1.png)
### 3.建立抽取和插入步骤    
![image.png](1-2.png)

### 4.编辑输入步骤
![image.png](1-3.png)
### 5.编辑插入步骤
如果两边字段都完全一致kettle会自动映射匹配
![image.png](1-4.png)

### 6.运行转换，启动所有步骤：
![image.png](1-5.png)
转换完成!
![image.png](1-6.png)
