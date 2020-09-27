---
title: 如何绘制矩形并通过在PyQt5中拖放来调整其形状
toc: true
mathjx: true
cover: /2020/06/27/如何绘制矩形并通过在PyQt5中拖放来调整其形状/head.png
tags:
  - Pyqt5
categories:
  - Python
  - Pyqt5
abbrlink: 52848
date: 2020-06-27 15:07:57
update:
---
由PyQt5创建的GUI上绘制一个矩形.
拖动时,通过鼠标移动来调整矩形形状.
释放鼠标左键时,确定矩形形状.

### 调用mouseMoveEvent函数
~~~python
import sys
from PyQt5 import QtWidgets, QtCore
from PyQt5.QtGui import QPainter

class MyWidget(QtWidgets.QWidget):
    def __init__(self):
        super().__init__()
        self.setGeometry(30,30,600,400)
        self.begin = QtCore.QPoint()
        self.end = QtCore.QPoint()
        self.show()

    def paintEvent(self, event):
        qp = QtGui.QPainter(self)
        br = QtGui.QBrush(QtGui.QColor(100, 10, 10, 40))  
        qp.setBrush(br)   
        qp.drawRect(QtCore.QRect(self.begin, self.end))       

    def mousePressEvent(self, event):
        self.begin = event.pos()
        self.end = event.pos()
        self.update()

    def mouseMoveEvent(self, event):
        self.end = event.pos()
        self.update()

    def mouseReleaseEvent(self, event):
        self.begin = event.pos()
        self.end = event.pos()
        self.update()
~~~
