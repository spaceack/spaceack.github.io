# Ubuntu20.04配置笔记


将中文目录改为英文：

    #debian/Ubuntu
    export LANG=en_US
    xdg-user-dirs-gtk-update
    export LANG=zh_CN

## 环境变量

```bash

# HISTORY history 日志添加时间戳；默认的2000条记录上限提高至99999条
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "
export HISTFILESIZE=99999

# node env 个人习惯把程序环境都放在 /opt 目录下
export PATH=$PATH:/opt/node/bin

# npm global env 
export PATH=~/.npm-global/bin:$PATH

# env jdk
JAVA_HOME=/opt/jdk
CLASSPATH=.:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH

# env golang
#export PATH=$PATH:/opt/go/bin
export GOROOT=/opt/go
export GOPATH=$HOME/goapp
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# redis env
export REDIS_HOME=/opt/redis
export PATH=$PATH:$REDIS_HOME/src

# hadoop 3.1.1 env
export HADOOP_HOME=/opt/hadoop-3.1.1
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

# hadoop 2.8.5 env
#export HADOOP_HOME=/opt/hadoop-2.8.5
#export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

# hbase env
export HBASE_HOME=/opt/hbase-2.1.1
export PATH=$PATH:$HBASE_HOME/bin

# mongodb
export PATH=/opt/mongodb/bin:$PATH

# RUST env
export PATH="$HOME/.cargo/bin:$PATH"

# android env
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/emulator

# QT env
export QTDIR=/opt/Qt/5.11.0/gcc_64/
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${QTDIR}/lib
export PATH=${QTDIR}/bin:${PATH}

alias postman='cd /opt/Postman/ && ./Postman'
alias datagrip='/opt/Datagrip/bin/datagrip.sh'
alias pycham='/opt/pycharm/bin/pycharm.sh'
alias idea='/opt/idea/bin/idea.sh'
alias webstorm='/opt/pycharm/bin/webstorm.sh'
alias spoon='sh /opt/di/spoon.sh'
alias elasticsearch='/opt/elasticsearch/bin/elasticsearch'
alias kibana='/opt/kibana/bin/kibana'
alias spoon='cd /opt/data-integration/ && ./spoon.sh'
alias robo3t='cd /opt/robo3t/bin/ && ./robo3t &'
alias justmd='cd /opt/justmd/ && ./justmd &'
alias vnote='/opt/vnote.AppImage &'

```

## 软件

### 常用工具

```bash
sudo -- sh -c '
sudo apt update
sudo apt upgrade

sudo apt install -y cmake;
sudo apt install -y git;
sudo apt install -y vim;
sudo apt install -y htop;
# 资源监控工具
sudo apt install -y glances;
sudo apt install -y tree;
sudo apt install -y curl;
sudo apt install -y aria2;
sudo apt install -y whois;
sudo apt install -y unrar;
sudo apt install -y lrzsz;
# 新立得软件包管理器
sudo apt install -y synaptic;
# 用于查找缺失的依赖库 apt-file search xxx.so
sudo apt install apt-file;
sudo apt install -y python3-distutils;
sudo apt-get install -y libmysqlclient-dev;
sudo apt install  -y python3-pip;
sudo apt install -y zlib1g-dev;
sudo apt install -y virtualenv;
sudo apt install -y nginx;
sudo apt install -y supervisor;
sudo apt install -y nmap;
# Common Lisp compiler
sudo apt-get install sbcl
# rime輸入法
sudo apt-get install ibus-rime
# 功能强大的词典， 需要配置第三方词典资源
sudo apt install -y goldendict;
# 文档格式转换
sudo apt install pandoc;
# 文本版本差异对比，合并工具 可视化的diff和merge 工具
sudo apt install -y meld;
# 远程桌面客户端
sudo apt install -y remmina;
# VCN 客户端 可远程控制 mac 桌面
sudo apt install -y krdc;
# 开源多媒体播放器， 感觉比VLC稳定。
sudo apt install -y smplayer;
sudo apt install -y qbittorrent;
# 密码管理器
sudo apt install -y keepassxc;
# 深度截图，国人开发，功能强大。 配合系统默认的带有延时截图的screenshot更佳。
sudo apt install -y deepin-screenshot;
# 深度桌面取色器
sudo apt install -y deepin-picker;
# 深度终端，支持多个ssh远程记录，好用！ 有个bug： 第一次远程需要手动连接。之后就可以一键登录远程了。
sudo apt install -y deepin-terminal;
# 通过管道监控数据进度
sudo apt install -y pv;
sudo apt install -y neofetch;
# 番茄时钟
sudo apt install gnome-shell-pomodoro;
# tweaks 界面功能增强，时钟显示秒，设置。
sudo apt-get install -y gnome-tweaks;
sudo pip3 install jupyterlab;
'
```
```bash
# 检查是否存在SSH秘钥
ls -al ~/.ssh
# 生成ssh-key 密钥对
ssh-keygen -t rsa -C "spaceack@qq.com"

# 将SSH key添加到ssh-agent：
ssh-add ~/.ssh/id_rsa

# 启用ssh
sudo apt update
sudo apt install openssh-server
sudo systemctl status ssh
# 禁用
sudo systemctl disable --now ssh

# 重启
sudo systemctl enable --now ssh

```

- npm非root全局安装权限
  ```
  mkdir ~/.npm-global
  npm config set prefix '~/.npm-global'
  # .bashrc 加入 export PATH=~/.npm-global/bin:$PATH
  source .bashrc
  ```

### 实用工具
**rime输入法**

`F4` 切换输入方案

```bash
sudo apt-get install ibus-rime
ibus restart
ibus engine rime
```

**Visual Studio Code**

**electerm**
  [electerm]( https://electerm.github.io/electerm )是一个跨平台的Terminal/SSH/SFTP客户端工具，同时支持Linux、MacOS、Windows，基于electron/ssh2/node-pty/xterm/antd/subx等开源组件。

**CherryTree**
  功能强大的树形笔记工具

**VNote**
  功能强大的 Vim 和 MarkDown 笔记工具

**DBeaver**
  通用数据库管理

 **Another Redis Desktop Manager**
[Another Redis Desktop Manager](https://github.com/qishibo/AnotherRedisDesktopManager)
**RDM**
  开源 Redis ® 管理工具
  `sudo snap install redis-desktop-manager`

**Zeal**
  (离线文档浏览器)[https://zealdocs.org/]
  ```
  sudo add-apt-repository ppa:zeal-developers/ppa
  sudo apt-get update
  sudo apt-get install zeal
  ```

**OBS(Open Broadcaster Software)**
开源视频录制与直播软件
```
sudo apt install ffmpeg
sudo add-apt-repository ppa:obsproject/obs-studio
sudo apt update
sudo apt install obs-studio
```
**ventoy**
多系统安装启动U盘制作工具  [Ventoy](https://github.com/ventoy/Ventoy)

**Peek**
超级好用的屏幕GIF动画录制器

```
sudo add-apt-repository ppa:peek-developers/stable
sudo apt-get update
sudo apt-get install peek
```

**yEd**
  绘制流程图[download](https://www.yworks.com/downloads#yEd)

**BlinkMind**
    [BlinkMind](https://github.com/awehook/blink-mind-package)
    简洁的思维导图工具

**KRDC**
  好用的远程桌面客户端

**GIMP**
  功能强大的图像处理和绘画软件

**Syncthing**
  多台设备双向文件同步工具

**VirtualBox**
  虚拟机

**Motrix**
多功能下载器 
特性： Async DNS BitTorrent Firefox3 Cookie GZip HTTPS Message Digest Metalink XML-RPC SFTP

**balenaEtcher**
  镜像写入工具
  支持的映像格式包括：img、iso、zip、bz2、dsk、etch、gz、hddimg、raw、xz等；

**calibre**
  电子书管理工具
  ```
  sudo -v && wget -nv -O- https://download.calibre-ebook.com/linux-installer.sh | sudo sh /dev/stdin
  ```
**永中Office**
体验不佳，暂不推荐~
推荐使用 LibreOffice

**robo3t**
MongoDB可视化工具


**OmegaT**
[开源计算机辅助翻译软件](https://omegat.org/)

**linux 字符终端 tty下显示中文**

```bash
sudo apt-get install fbterm -y
sudo gpasswd -a $USER video
sudo chmod u+s /usr/bin/fbterm
```


### 趣味软件

**sonic-pi**
  通过编码的音乐合成器

**figlet**

**neofetch**
![neofetch.png](neofetch.png)

**Godot**
开源游戏引擎

**命令行查询天气**
 curl http://wttr.in/

