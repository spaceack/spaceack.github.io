# Git常用命令


### create a new repository on the command line

```sh
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:user/project_name.git
git push -u origin master
```

### push an existing repository from the command line

```sh
git remote add origin git@github.com:user/project_name.git
git push -u origin master
```

### tag

```bash
# 查看
git tag
# 在某个 commit 打 tag
git tag tag_name commit_id
# 本地 tag 推到线上
git push origin tag_name
# 删除本地 tag
git tag -d tag_name
# 删除本地，然后删除线上tag
git push origin :refs/tags/tag_name
# 添加tag 并推送到远程
git tag tag_name -m "tag_msg"
git push origin tag_name

```

### commit

```bash
# 统计提交次数
git log | grep "^Author: Spaceack"|wc -l
# 统计合并次数
git log | grep "^Merge" | wc -l
# 修改当前分支最新的一次提交。commit的 hash 不变. 避免有多个commit的情况。
git commit --amend
```

### git log

使用 `pretty` 格式化参数获取更详细的信息

`git log --pretty="%h %p %t  %cn %an %s "`

```bash
选项   说明
%H    提交对象（commit）的完整哈希字串
%h    提交对象的简短哈希字串
%T    树对象（tree）的完整哈希字串
%t    树对象的简短哈希字串
%P    父对象（parent）的完整哈希字串
%p    父对象的简短哈希字串
%an   作者（author）的名字
%ae   作者的电子邮件地址
%ad   作者修订日期（可以用 -date= 选项定制格式）
%ar   作者修订日期，按多久以前的方式显示
%cn   提交者(committer)的名字
%ce   提交者的电子邮件地址
%cd   提交日期
%cr   提交日期，按多久以前的方式显示
%s    提交说明
```

### git config

```bash
git config --global user.name "spaceack"
git config --global user.email "spaceack@qq.com"
```

#### 忽略文件权限的变更

```bash
git config core.filemode false //当前版本库
git config --global core.filemode false //全局
```

### proxy

#### 设置HTTP代理

```bash
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

#### 取消代理

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### submodule

#### 项目不完整， submodule 目录为空

```bash
# 进入到 submodule 目录
git submodule update --init --recursive
```

