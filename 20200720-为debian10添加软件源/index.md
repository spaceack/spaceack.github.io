# 为debian10添加软件源

Debian 10添加163软件源：
```
vi /etc/apt/source.list
deb http://mirrors.163.com/debian/ buster main contrib non-free
deb http://mirrors.163.com/debian/ buster-updates main contrib non-free
deb http://mirrors.163.com/debian-security/ buster/updates main contrib non-free
:wq
```
其它debian发行版本同理，更改代号即可，历史代号可到官网查看 [debian.org](https://www.debian.org/releases/index.zh-cn.html)


    下一代 Debian 正式发行版的代号为 "bullseye" — 发布时间尚未确定
    Debian 10（"buster"） — 当前的稳定版（stable）
    Debian 9（"stretch"） — 旧的稳定版（oldstable）
    Debian 8（"jessie"） — 更旧的稳定版（oldoldstable）
    Debian 7（"wheezy"） — 被淘汰的稳定版
    Debian 6.0（"squeeze"） — 被淘汰的稳定版
    Debian GNU/Linux 5.0（"lenny"） — 被淘汰的稳定版
    Debian GNU/Linux 4.0（"etch"） — 被淘汰的稳定版
    Debian GNU/Linux 3.1（"sarge"） — 被淘汰的稳定版
    Debian GNU/Linux 3.0（"woody"） — 被淘汰的稳定版
    Debian GNU/Linux 2.2（"potato"） — 被淘汰的稳定版
    Debian GNU/Linux 2.1（"slink"） — 被淘汰的稳定版
    Debian GNU/Linux 2.0（"hamm"） — 被淘汰的稳定版



