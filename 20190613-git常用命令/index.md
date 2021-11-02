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
```

###  git config
```
git config --global user.name "spaceack"
git config --global user.email "spaceack@qq.com"
```

### proxy

#### 设置HTTP代理
```
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

#### 取消代理

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### submodule

#### 项目不完整， submodule 目录为空
```
# 进入到 submodule 目录
git submodule update --init --recursive
```

