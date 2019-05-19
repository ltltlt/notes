# some problems have been solved

## 部分应用在rofi drun中不显示，比如firefox

发现非rofi的问题，而是此应用的desktop文件中有OnlyShowIn=Unity项，导致其只在Unity桌面环境中显示在rofi drun中。

## 使用fcitx-configtool中配置禁用双shift切换输入法，每次注销或重启后总是被修改回来

发现fcitx-configtool确实会修改~/.config/fcitx/config文件，但每次注销后此文件被重写回原先配置。使用auditd监视文件的修改，发现有个程序sogou-qpanel会在注销后写此文件。最后，将默认输入法改为rime即可解决。