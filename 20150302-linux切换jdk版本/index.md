# Linux切换jdk默认版本


1. 安装多个jdk版本

`sudo yum install java-1.8.0-openjdk-devel`

`sudo yum install java-11-openjdk-devel`

2. 查看当前默认版本
`java -version`

3. 切换默认版本
`sudo alternatives --config java`
