# VScode常用插件及配置

### 流程图绘制
`Draw.io`
新建 `.dio` 的文件即可编辑。

### TabNine
提供多种代码的自动补全

### 配置远程编辑
1. 安装插件 `remote-browser`
2. 文件→首选项→设置
3. 搜索 `remoteBrowser.connectionOptions`， 编辑 `setting.json`
4. 添加:
  ```json
      #添加远程登录的主机地址、用户账号及访问的端口即可
      "remoteBrowser.connectionOptions": {

          "host": "10.x.x.11",  

          "username": "admin", 

          "port": 22

      },
  ```
  5. **ctrl+shift+p**, 使用命令 `remote-browser:Connect` 链接远程。
  6. VScode右下角将提示已远程登录成功，`remote-browser:Connected`
  7. 文件浏览界面 `REMOTE BROWSER` 查看远程目录文件。

###  代码对齐插件 Code alignment
1. 安装插件 `Code alignment`
2. 选中需要对齐的代码
3. `F1` 选择需要对齐的功能：支持等号对齐(`Ctrl `+ `=`)， 注释对齐等。

### 浏览pdf
1. 安装插件 `vscode-pdf`

### 中文插件
1. Chinese(Simplified)

### Java开发
`Extension Pack for Java`

`Spring Boot Extension Pack`
