# git常见问题处理

### git status 中文显示异常

中文名等特殊符号被转义，不易读。
使用下面命令可正常显示中文。
`git config --global core.quotepath false`

### 查看项目中某个人的代码数
  ```
  git log --author="Spaceack" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' 

  added lines: 1280, removed lines: 198, total lines: 1082
  ```

### 合并多次commit
  例如要合并 最新的三次提交,将后两个提交合并到第一个：
  ```
  git rebase -i HEAD~3
  
  选择pick操作，git会应用这个补丁，以同样的提交信息（commit message）保存提交
  选择reword操作，git会应用这个补丁，但需要重新编辑提交信息
  选择edit操作，git会应用这个补丁，但会因为amending而终止
  选择squash操作，git会应用这个补丁，但会与之前的提交合并
  选择fixup操作，git会应用这个补丁，但会丢掉提交日志
  选择exec操作，git会在shell中运行这个命令
  ```
  然后将后两个提交的`pick` 改为 `squash`

  


### 解决Git冲突 Please move or remove them before you can merge. Aborting

  ```
  x  -----删除忽略文件已经对git来说不识别的文件
  d  -----删除未被添加到git的路径中的文件
  f  -----强制运行

  git clean  -d  -fx ""
  ```

### 您的分支和 'origin/master' 出现了偏离
  ```
  git status
  位于分支 master
  您的分支和 'origin/master' 出现了偏离，
  并且分别有 1 和 1 处不同的提交。
    （使用 "git pull" 来合并远程分支）

  想要丢弃你所有的本地改动与提交，可以到服务器上获取最新的版本并将你本地主分支指向到它：

  git fetch origin
  git reset --hard origin/master
  ```
### 拒绝合并无关历史
  ```
  fatal: refusing to merge unrelated histories

  git pull origin hexo --allow-unrelated-histories
  ```

### git pull 强制覆盖本地文件
  ```
  git fetch --all
  git reset --hard origin/master

  ``` 
### pull时，忽略变更的文件：
  ```
  git stash
  git pull
  git stash pop
  ```

## GitHub

### 更新 fork 分支

  ```
 - RP: spaceack/awesomeproject
 - Fork: xxxx/awesomeproject

  git clone https://github.com/spaceack/awesomeproject.git
  cd awesomeproject
  git remote add upstream https://github.com/spaceack/myframework.git

- Update:
  git fetch upstream
  git rebase upstream/master

  git push
  git push --tags
  ```

