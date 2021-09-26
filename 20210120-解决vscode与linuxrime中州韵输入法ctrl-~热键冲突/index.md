# 解决VScode与LinuxRime中州韵输入法CTRL+~热键冲突

 `Ctrl + ~`  为 `VScode` 调出集成终端的快捷键。同时也是 `Rime` 输入法方案菜单的快捷键。`VScode` 的快捷键会被覆盖。

因为 `rime` 热键 `F4` 与 `CTRL + ~` 作用相同， 这里去掉 `CTRL + ~` 热键：

`sudo vim /usr/share/rime-data/default.yaml`

找到 `switcher` 下的 `- Control+grave` 删除该行即可，重启或重新部署生效。

文档参考： 

[Rime 定製指南
](https://github.com/rime/home/wiki/CustomizationGuide#%E4%B8%80%E4%BE%8B%E5%AE%9A%E8%A3%BD%E5%96%9A%E5%87%BA%E6%96%B9%E6%A1%88%E9%81%B8%E5%96%AE%E7%9A%84%E5%BF%AB%E6%8D%B7%E9%8D%B5)

[Rime 必知必会](https://github.com/rime/home/wiki/RimeWithSchemata#%E5%BF%85%E7%9F%A5%E5%BF%85%E6%9C%83)

---
欢迎访问 `spaceack.com`
