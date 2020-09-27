---
title: pyqt5插入图片
toc: true
mathjx: true
cover: /2020/05/25/pyqt5插入图片/head.png
tags:
  - Pyqt5
categories:
  - Python
  - Pyqt5
abbrlink: 40689
date: 2020-05-25 20:39:56
update:
---
### designer中图片资源插入
![](1.png)
如修改lable控件的背景图，选择资源，导入qrc文件。

qrc文件格式编辑
~~~
<RCC>
  <qresource>
    <file>1.jpeg</file>
    <file>ico/add.png</file>
    <file>ico/add_to_list.png</file>
    <file>ico/center_volume.png</file>
    <file>ico/cycle.png</file>
    <file>ico/cycle_one.png</file>
    <file>ico/de.png</file>
    <file>ico/delete.png</file>
    <file>ico/disc.png</file>
    <file>ico/downloads.png</file>
    <file>ico/high_volume.png</file>
    <file>ico/kk-music.ico</file>
    <file>ico/logo.png</file>
    <file>ico/music.png</file>
    <file>ico/my_playlist.png</file>
    <file>ico/no_volume.png</file>
    <file>ico/on.png</file>
    <file>ico/open_folder.png</file>
    <file>ico/random.png</file>
    <file>ico/small_volume.png</file>
    <file>ico/song.png</file>
    <file>ico/start.png</file>
    <file>ico/搜索.png</file>
    <file>ico/网易云音乐.png</file>
    <file>ico/stop.png</file>
    <file>ico/音量 .png</file>
  </qresource>
</RCC>
~~~
再使用命令
~~~
pyrcc5 xx.qrc -o xx.py
~~~
并在ui文件转换的py文件中导入该文件，作为图片资源文件。
