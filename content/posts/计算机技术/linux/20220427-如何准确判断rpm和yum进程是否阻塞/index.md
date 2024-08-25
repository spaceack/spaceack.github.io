---
title: 如何准确判断rpm,yum,dnf进程是否阻塞[运行]？
date: 2022-04-27 21:00:00
update: 2022-04-27 21:00:00
author: Spaceack
categories: ["Linux"]
tags: 
  - dnf
  - yum
  - rpm
  - pid
  - lock
---

如果你是一个程序员，你会知道，`rpm`, `yum`, `dnf`这三个进程是否运行，如果运行，是否阻塞？

## 简单常用但不严谨的做法

一个简单方法是通过 `ps -ef | grep rpm | grep -v grep` 命令来判断。

但这种方法有两个问题：
1. 不严谨，当进程名中还有三个命令名字的某个子字符串时，就会误判。要不就是需要繁琐的规则去过滤，繁琐意味着引入更多的错误。

2. 仅能够判断有进程在运行，但并不一定会阻塞。例如当进行到用户确认这一步时，虽然有此进程，但依然可以再开个终端安装其它程序。

![install_rust](481697212615314.png)

![ps -ef|grep dnf](380132136941065.png)

## 一种可行的做法的思考及验证过程

### 思考

这时我们可以思考，`rpm`,`yum`,`dnf`安装程序本身是如何保证进程互斥的呢？

不难发现，当我们同时开两个终端执行相同的安装命令时，会有一个终端报错`Waiting for process with pid 67 to finish.` 等待某个pid进程结束。

自然想到是某种锁机制在发挥作用，再联想到Linux的一切都可以是文件。那么这个`锁`也极有可能是个文件。

通过`搜索引擎`很容易发现·`yum`的 pid文件进程锁 的路径 `/var/run/yum.pid`。

### 验证

#### 验证程序

由于安装程序可能很快完成，来不及看到锁文件。所以我们可以快速实现一个简易的验证程序：

```python
import os
import shutil
import time
import subprocess
def runcmd(command):
    ret = subprocess.run(command, shell=True, timeout=60, stdout=subprocess.PIPE, stderr=subprocess.PIPE, encoding="utf-8")
    if ret.returncode == 0:
        return ret.stdout
    else:
        print(str(ret))

def check_dnf_lock_pid():
    """若进程锁文件存在，有其它正在执行的安装程序， 返回其它进程信息 或 True; 否则返回 None
        早期系统 yum 包安装程序通过 '/var/run/yum.pid' 判断。
        新系统   dnf 包安装程序通过以下4个进程锁文件判断。
            "/var/cache/dnf/download_lock.pid",
            "/var/cache/dnf/metadata_lock.pid",
            "/var/lib/dnf/rpmdb_lock.pid",
            "/var/log/log_lock.pid",
    """
    lock_pid_path = [
        "/var/cache/dnf/download_lock.pid",
        "/var/cache/dnf/metadata_lock.pid",
        "/var/lib/dnf/rpmdb_lock.pid",
        "/var/log/log_lock.pid",
        "/var/run/yum.pid",
        "/var/lib/rpm/.rpm.lock",
    ]
    for pid_path in lock_pid_path:
        if os.path.exists(pid_path):
            if "pid" in pid_path:
                cat_command = "cat %s" % (pid_path)
                pid = runcmd(cat_command)
                pid = pid.replace("\n","") if pid else None
            if ".rpm.lock" in pid_path:
                fuser_command = "fuser %s" % (pid_path)
                pid = runcmd(fuser_command)
                if isinstance(pid, str):
                    pid = pid.replace(" ", "").replace("\n", "") if pid else None
                else:
                    pid = pid.stdout.decode("utf-8").replace(" ", "").replace("\n", "") if pid else None
            if pid and pid.isdigit():
                run_time_c = "ps -p %s -o etimes=|awk '{print $1}'" % (pid)
                run_time = runcmd(run_time_c)
                run_time = run_time.replace("\n","") if run_time else None
            if pid and run_time and pid.isdigit() and run_time.isdigit():
                return "当前安前程序进程%s正在执行，已运行%s秒，请等待结束后再尝试。等待进程%s" % (pid, run_time, pid_path,)
            if pid and pid.isdigit():
                return "当前安装程序进程%s正在执行，请等待结束后再尝试。等待进程%s" % (pid, pid_path,)
while True:
    c = check_dnf_lock_pid()
    print(c)
```

#### 山穷水尽疑无路

但很遗憾，并没有发现这个锁程序。

不过我们想到最新系统 `yum` 已被 `dnf` 所取代。那么可能网上都是一些过时的信息。

#### 柳暗花明又一村

这时最直接的方法当然是去撸源码咯！
`git clone https://github.com/rpm-software-management/dnf.git` 走起

通过`pid`关键字快速定位 `grep -inR pid`。

果然，很容易验证了我们正确的猜想
![dnf-pid-path](161477200889469.png)


#### 再次验证

#### rpm 的检验

`fuser /var/lib/rpm/.rpm.lock`
