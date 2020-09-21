---
title: Pyqt5嵌套窗口切换
toc: true
mathjx: true
cover: /2019/02/07/Pyqt5嵌套窗口切换/head.png
tags:
  - Python
categories:
  - Python
  - Pyqt5
abbrlink: 16895
date: 2019-02-07 11:53:03
update:
---

### designer设计
使用QstackerWidget 进行多界面切换
![](designer设计图.png)

### Ui代码
~~~Python
from PyQt5 import QtCore, QtGui, QtWidgets


class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(800, 600)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.B1 = QtWidgets.QPushButton(self.centralwidget)
        self.B1.setGeometry(QtCore.QRect(50, 180, 75, 23))
        self.B1.setObjectName("B1")
        self.B2 = QtWidgets.QPushButton(self.centralwidget)
        self.B2.setGeometry(QtCore.QRect(50, 280, 75, 23))
        self.B2.setObjectName("B2")
        self.stackedWidget = QtWidgets.QStackedWidget(self.centralwidget)
        self.stackedWidget.setGeometry(QtCore.QRect(200, 60, 551, 421))
        self.stackedWidget.setObjectName("stackedWidget")
        self.page1 = QtWidgets.QWidget()
        self.page1.setObjectName("page1")
        self.frame1 = QtWidgets.QFrame(self.page1)
        self.frame1.setGeometry(QtCore.QRect(180, 110, 221, 141))
        self.frame1.setStyleSheet("background-color: rgb(255, 255, 127);")
        self.frame1.setFrameShape(QtWidgets.QFrame.StyledPanel)
        self.frame1.setFrameShadow(QtWidgets.QFrame.Raised)
        self.frame1.setObjectName("frame1")
        self.stackedWidget.addWidget(self.page1)
        self.page2 = QtWidgets.QWidget()
        self.page2.setObjectName("page2")
        self.frame2 = QtWidgets.QFrame(self.page2)
        self.frame2.setGeometry(QtCore.QRect(149, 89, 251, 191))
        self.frame2.setStyleSheet("background-color: rgb(255, 0, 127);")
        self.frame2.setFrameShape(QtWidgets.QFrame.StyledPanel)
        self.frame2.setFrameShadow(QtWidgets.QFrame.Raised)
        self.frame2.setObjectName("frame2")
        self.stackedWidget.addWidget(self.page2)
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 800, 23))
        self.menubar.setObjectName("menubar")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "MainWindow"))
        self.B1.setText(_translate("MainWindow", "一号色"))
        self.B2.setText(_translate("MainWindow", "二号色"))

~~~

### 控制代码
~~~Python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow
from win import Ui_MainWindow


class MainWin(QMainWindow, Ui_MainWindow):
    def __init__(self, parent=None):
        super(MainWin, self).__init__(parent)
        self.setupUi(self)

        self.B1.clicked.connect(lambda: self.stackedWidget.setCurrentIndex(0))  # 点击按钮1展示第一个frame
        self.B2.clicked.connect(lambda: self.stackedWidget.setCurrentIndex(1))  # 点击按钮2展示第二个frame


if __name__ == '__main__':
    app = QApplication(sys.argv)
    win = MainWin()
    win.show()
    sys.exit(app.exec_())
~~~

### 效果图
![](b1.png)
![](b2.png)
