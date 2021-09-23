---
title: 玩转midi:使用vmpk在Gnu/Linux下弹钢琴
date: 2017-10-23 12:00:00
update: 2017-10-23 12:00:00
author: Spaceack
categories: ["Linux"]
tags:
  - Linux
  - tools
  - midi
---

### 安装
```bash
sudo apt-get install vmpk
sudo apt-get install timidity
```
### 配置

- 依次打开　QjackCTL，Qsynth(打开后其它音乐软件会禁声),  VMPK,
#### 配置Qsynth:
  - Soundfonts　→　Open 加载sf2文件，(一般路径为/usr/share/sounds/sf2/FluidR3_GM.sf2) 
 

#### 配置QjackCTL: 
  - 1.Start
  - 2.Connect
    - Audio: qsynth match system
    - ALSA: VMPK Output match FLUID Synth(21225)

#### 配置VMPK
  - Edit → Connections → Output MIDI Connection: FLUID Synth → OK

- 尽情在键盘上挥洒音符吧：）

---

  - 软件禁声恢复: quit Qsynth, STOP QjackCTL

---

[ubuntu中文论坛ＶＭＰK 配置](http://forum.ubuntu.org.cn/viewtopic.php?p=3035326)
