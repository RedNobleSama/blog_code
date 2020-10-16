---
title: PyQt的Led控件显示库
toc: true
mathjx: true
cover: /2020/10/16/PyQt的Led控件显示库/head.png
tags:
  - Python
categories:
  - Python
  - Pyqt5
abbrlink: 9043
date: 2020-10-16 14:30:09
update:
---
### 控件说明
在Github上，偶然发现了一个基于PyQt5的第三方Led指示灯控件库，使用起来非常方便，控件外观也比较漂亮，更难能可贵的是作者源代码写得比较简洁，仅仅才约200行左右，可以作为一个在PyQt中写自定义控件方法的非常好的学习例子。这个控件具有以下特点：
1. 提供了3种外形可供选择，分别为：'capsule', 'circle', 'rectangle'等
2. 提供了7种颜色可供选择，分别为：'blue', 'green', 'orange', 'purple', 'red', 'yellow'等

### 安装使用
~~~
pip install pyqt-led
~~~
在代码中使用时，只需使用以下语句导入该库的LED类即可：
~~~python 
from pyqt_led import led
~~~
在该库中，提供了几个主要的方法函数，包括set_on_color、set_off_color、set_shape、turn_on、turn_off等函数，分别设置Led的开/关颜色、形状及设置开、关状态等。

### 使用案例
~~~pythonython
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import *
import numpy as np
from pyqt_led import Led

class Demo(QWidget):
    def __init__(self, parent=None):
        QWidget.__init__(self, parent)
        self._shape = np.array(['capsule', 'circle', 'rectangle'])
        self._color = np.array(['blue', 'green', 'orange', 'purple', 'red',
                                'yellow'])
        self._layout = QGridLayout(self)
        self._create_leds()
        self._arrange_leds()
        self.resize(400, 200)
        self.setWindowTitle('pyqt-led Demo')

    def keyPressEvent(self, e):
        if e.key() == Qt.Key_Escape:
            self.close()

    def _create_leds(self):
        for s in self._shape:
            for c in self._color:
                exec('self._{}_{} = Led(self, on_color=Led.{}, shape=Led.{}, build="debug")'
                     .format(s, c, c, s))
                exec('self._{}_{}.setFocusPolicy(Qt.NoFocus)'.format(s, c))
                exec('self._{}_{}.turn_on(True)'.format(s, c))

    def _arrange_leds(self):
        for r in range(3):
            for c in range(6):
                exec('self._layout.addWidget(self._{}_{}, {}, {}, 1, 1, \
                      Qt.AlignCenter)'
                     .format(self._shape[r], self._color[c], r, c))

app = QApplication(sys.argv)
demo = Demo()
demo.show()
sys.exit(app.exec_())
~~~
运行这个程序后，即可出现本文开头所示的在一个窗口上显示了不同形状、不同颜色的Led的窗口。默认运行时显示的为全亮状态，当设置为全灭状态时，如下图所示：

在代码中，Led类实例化时，其构造函数原型如下所示：
~~~python
Led(parent, on_color=green, off_color=black, shape=rectangle, build='release')
~~~

可传入父窗口、初始的亮颜色、灭颜色、形状及状态参数。

对于需要在程序开发中使用指示灯的情况，如显示各种程序的运行状态等，应该说这是一个比较实用的库了。当然，由于源代码比较简单，你也可以在其基础上进一步修改订制，比如添加其它形状、颜色等选项设置，以满足自己的项目使用要求。