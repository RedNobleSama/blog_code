---
title: pyqt5文本高亮
toc: true
mathjx: true
cover: /2020/04/15/pyqt5文本高亮/head.png
tags:
  - Python
categories:
  - Python
  - Pyqt5
abbrlink: 32696
date: 2020-04-15 16:12:03
update:
---

~~~python
class MainWindow(QMainWindow, Ui_MainWindow):
    ......
    ......
    ......
    def file(self, Qmodelidx):
        with open(self.model.filePath(Qmodelidx), 'r', encoding='utf-8', errors='ignore') as f:
            # 打开utf-8格式的文件，遇到出错进行忽略
            # filename返回值为('C:/PyQt5/PyQt1.py', 'All Files (*)')是一个元组，但with...open...只需要提供一个文件的地址而已,故输入参数filename[0]
            # 读取所选择的文件名，并将文本编辑小部件的内容设置为文件读取的内容。这里提一下使用with语句来自动帮我们调用close()
            # 方法，避免由于文件读写时产生IOError，导致close()
            # 不会调用，需要try...finally来实现的不便。
            data = f.read()  # 将文件读取出来

        self.textEdit.setPlainText(data)  # 将读取到的内容放到textEdit小部件中
        li = ['def', 'import', 'from', 'print'] # 高亮的关键字
        for i in li:
            text = i
            if not text:
                return
            col = QColor(250, 97, 0)  # 置为橙色
            fmt = QTextCharFormat()
            fmt.setForeground(col)
            # 先把光标移动到开头
            self.textEdit.moveCursor(QTextCursor.Start)
            while self.textEdit.find(text, QTextDocument.FindWholeWords):  # 查找所有文字
                self.mergeFormatOnWordOrSelection(fmt)

    def mergeFormatOnWordOrSelection(self, format):
        cursor = self.textEdit.textCursor()
        if not cursor.hasSelection():
            cursor.select(QTextCursor.WordUnderCursor)
        cursor.mergeCharFormat(format)
        self.textEdit.mergeCurrentCharFormat(format)
~~~
