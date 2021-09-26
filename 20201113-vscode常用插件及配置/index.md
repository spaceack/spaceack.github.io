# VScode常用插件及配置


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

### 浏览pdf
1. 安装插件 `vscode-pdf`
