---
layout: post
title: PyQt4 下的第一个程序
tags:
- Chinese
- PyQt
- PyQt4
- Python
---

本文翻译自 <a href="http://zetcode.com/tutorials/pyqt4/" target="_blank">Zetcode 的 PyQt4 教程</a>

在 PyQt4 教程的这一部分，我们将学习一些基本的功能。

<!--more-->

## 简单的例子

这个例子非常简单。它只显示了一个小窗口，但是我们仍然可以对它做很多事。我们可以重新设置大小，最大化，最小化。这需要很多代码实现。一些人已经将代码封装。因为在大多数应用程序中都要使用，没有必要一次又一次地重复编写代码。所以这部分对程序员是隐藏的。PyQt4 是一个高级的工具集。如果我们用更底层的工具集编码，下面例子中的代码将十分冗长。

{% highlight python %}

#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PyQt4 tutorial 

In this example, we create a simple
window in PyQt4.

author: Jan Bodnar
website: zetcode.com 
last edited: October 2011
"""

import sys
from PyQt4 import QtGui


def main():
    
    app = QtGui.QApplication(sys.argv)
	
    w = QtGui.QWidget()
    w.resize(250, 150)
    w.move(300, 300)
    w.setWindowTitle('Simple')
    w.show()
	
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()

{% endhighlight %}

上面的代码在屏幕上显示了一个小窗口。

{% highlight python %}

import sys
from PyQt4 import QtGui

{% endhighlight %}

这里引入了一些必须的东西。基础的 GUI 部件位于`QtGui`模块下。

{% highlight python %}

app = QtGui.QApplication(sys.argv)

{% endhighlight %}

每一个 PyQt4 应用程序必须创建一个应用程序对象。这个应用程序对象位于`QtGui`模块。`sys.argv`是一个列表，包含了来自命令行的参数。Python 脚本可以在 Shell 中运行。这是一种控制我们脚本启动的方式。

{% highlight python %}

w = QtGui.QWidget()

{% endhighlight %}

`QtGui.QWidget`部件是 PyQt4 中哦你那个所以用户接口对象的基类。我们提供了`QtGui.QWidget`的默认构造函数。默认购函数没有父类。没有父类的部件被称为窗口。

{% highlight python %}

w.resize(250, 150)

{% endhighlight %}

`resize()`方法重新设置了部件的大小。它宽 250px，高 150px。

{% highlight python %}

w.move(300, 300)

{% endhighlight %}

`move()`方法将部件移动到屏幕上横坐标为 300，纵坐标为 300 的位置。

{% highlight python %}

w.setWindowTitle('Simple')

{% endhighlight %}

这里设置了窗口的标题。标题显示在标题栏上。

{% highlight python %}

w.show()

{% endhighlight %}

`show()`方法在屏幕上显示了部件。部件先在内存里创建，然后在屏幕上显示。

{% highlight python %}

sys.exit(app.exec_())

{% endhighlight %}

最后，我们进入程序的主循环。事件处理从这时开始。主循环接收从窗口系统发送的事件，分解到应用程序的各个部件。如果我们调用`exit()`或主部件被关闭，主循环结束。`sys.exit()`方法确保能干净地退出。运行环境将会被告知应用程序如何退出。

`exec_()`方法名中有一个下划线，这是因为`exec`是 Python 的关键字。因此使用`exec_()`代替。

![Simple程序的截图](http://zetcode.com/img/gui/pyqt4/simple.png "Simple")

图片：Simple

## 应用程序图标

应用程序图标是一个小图片，通常显示在窗口上方标题栏的左边。我们将在在接下来的例子中演示在 PyQT4 中如何显示应用程序图标。我们也将介绍一些新的方法。

{% highlight python %}

#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PyQt4 tutorial 

This example shows an icon
in the titlebar of the window.

author: Jan Bodnar
website: zetcode.com 
last edited: October 2011
"""

import sys
from PyQt4 import QtGui


class Example(QtGui.QWidget):

	def __init__(self):
        super(Example, self).__init__()
	    
	    self.initUI()
	    
	def initUI(self):
	    
	    self.setGeometry(300, 300, 250, 150)
	    self.setWindowTitle('Icon')
	    self.setWindowIcon(QtGui.QIcon('web.png'))        
	
	    self.show()
	    
def main():
	
	app = QtGui.QApplication(sys.argv)
	ex = Example()
	sys.exit(app.exec_())


if __name__ == '__main__':
	main()

{% endhighlight %}

之前的例子是按照面向过程的风格编写的。Python 同时支持面向过程和面向对象的编程风格。使用 PyQT4 编程意味着使用面向对象的风格编程。

{% highlight python %}

class Example(QtGui.QWidget):

	def __init__(self):
    	super(Example, self).__init__()

{% endhighlight %}

在面向对象编程中最重要的三个东西是类，数据和方法。这里我们创建了一个名为`Example`的类。其继承自`QtGui.QWidge`类。这意味着，我们必须调用两个构造函数。第一个是`Example`类的，第二个是继承的那个类的。`super()`方法返回`Example`的父类对象，然后我们调用它的构造函数。`__init__()`方法在 Python 中即构造函数。

{% highlight python %}

self.initUI() 

{% endhighlight %}

GUI的创建被分配到initUI()方法中。

{% highlight python %}

self.setGeometry(300, 300, 250, 150)
self.setWindowTitle('Icon')
self.setWindowIcon(QtGui.QIcon('web.png')) 

{% endhighlight %}

所有的三个方法都继承自`QtGui.QWidget`类。`setGeometry()`做了两件事。它定位了窗口在屏幕上的位置并设置了窗口的大小。头两个参数是窗口的横纵坐标。第三个、第四个分别为窗口的宽度和高度。事实上，它将`resize()`和`move()`合并到一个方法中。最后一个方法设置了应用程序的图标。我们创建了`QtGui.QIcon`对象。`Qt.QIcon`接收所要显示图标的路径。

{% highlight python %}

def main():

	app = QtGui.QApplication(sys.argv)
	ex = Example()
	sys.exit(app.exec_())


if __name__ == '__main__':
	main()

{% endhighlight %}

启动代码放置在`main()`方法里。这是 Python 的习惯。

![Icon程序的截图](http://zetcode.com/img/gui/pyqt4/icon.png "Icon")

图片：Icon

## 显示工具栏

我们可以为任何一个部件提供气球式的帮助。

{% highlight python %}

#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PyQt4 tutorial 

This example shows a tooltip on 
a window and a button

author: Jan Bodnar
website: zetcode.com 
last edited: October 2011
"""

import sys
from PyQt4 import QtGui


class Example(QtGui.QWidget):
	
    def __init__(self):
	    super(Example, self).__init__()
	    
	    self.initUI()
	    
	def initUI(self):
	    
	    QtGui.QToolTip.setFont(QtGui.QFont('SansSerif', 10))
	    
	    self.setToolTip('This is a <b>QWidget</b> widget')
	    
	    btn = QtGui.QPushButton('Button', self)
	    btn.setToolTip('This is a <b>QPushButton</b> widget')
	    btn.resize(btn.sizeHint())
	    btn.move(50, 50)       
	    
	    self.setGeometry(300, 300, 250, 150)
	    self.setWindowTitle('Tooltips')    
	    self.show()
	    
def main():
	
	app = QtGui.QApplication(sys.argv)
	ex = Example()
	sys.exit(app.exec_())


if __name__ == '__main__':
	main()

{% endhighlight %}

在这个例子中，我们为两个 PyQt4 部件提供了提示信息。

{% highlight python %}

QtGui.QToolTip.setFont(QtGui.QFont('SansSerif', 10))

{% endhighlight %}

这个静态方法为提示信息设置了字体。我们使用 10px 的 SansSerif 字体。

{% highlight python %}

self.setToolTip('This is a <b>QWidget</b> widget')

{% endhighlight %}

我们调用`seTooltip()`方法创建提示信息。我们可以使用富文本格式。

{% highlight python %}

btn = QtGui.QPushButton('Button', self)
btn.setToolTip('This is a <b>QPushButton</b> widget')

{% endhighlight %}

我们创建了一个按钮并设置了提示信息。

{% highlight python %}

	btn.resize(btn.sizeHint())
	btn.move(50, 50)

{% endhighlight %}

按钮被重新设置大小，移动到窗口上。`sizeHint()`方法为按钮给出了推荐的大小。

![Tooltip程序的截图](http://zetcode.com/img/gui/pyqt4/tooltip.png "Tooltip")

图片：Tooltip

## 关闭窗口

关闭窗口最简单的方法就是点击标题栏上的X。在下面的例子中，我们将展示如何在代码中关闭我们的窗口。我们将简单地介绍信号和槽。

下面是在例子中我们要用到的`QtGui.QPushButton`的构造函数。

{% highlight python %}

QPushButton(string text, QWidget parent = None)

{% endhighlight %}

`text`参数是一个在按钮上显示的文本。`parent`是一个我们放置在按钮上的部件。在马的例子中是`QtGui.QWidget`。

{% highlight python %}

#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PyQt4 tutorial 

This program creates a quit
button. When we press the button,
the application terminates. 

author: Jan Bodnar
website: zetcode.com 
last edited: October 2011
"""

import sys
from PyQt4 import QtGui, QtCore


class Example(QtGui.QWidget):
    
    def __init__(self):
    	super(Example, self).__init__()
    	
    	self.initUI()
        
    def initUI(self):               
    	
    	qbtn = QtGui.QPushButton('Quit', self)
    	qbtn.clicked.connect(QtCore.QCoreApplication.instance().quit)
    	qbtn.resize(qbtn.sizeHint())
    	qbtn.move(50, 50)       
    	
    	self.setGeometry(300, 300, 250, 150)
    	self.setWindowTitle('Quit button')    
    	self.show()
    	
def main():
	
    app = QtGui.QApplication(sys.argv)
	ex = Example()
	sys.exit(app.exec_())


if __name__ == '__main__':
	main()

{% endhighlight %}

在这个例子中我们创建了一个退出按钮。只要在窗口中点击，应用程序就终止了。

{% highlight python %}

from PyQt4 import QtGui, QtCore

{% endhighlight %}

在这个例子中，我们将使用`QtCore`模块中的一个对象。

{% highlight python %}

qbtn = QtGui.QPushButton('Quit', self)

{% endhighlight %}

我们创建了一个按钮。这个按钮是`QtGui.QPushButton`类的一个实例。构造函数的第一个参数是按钮的标签。第二个参数是父部件。父部件是`Example`部件，继承自`QtGui.QWidget`。

{% highlight python %}

qbtn.clicked.connect(QtCore.QCoreApplication.instance().quit)

{% endhighlight %}

PyQt4中的事件处理系统按照信号-槽的机制构建。如果我们点击按钮，发出`clicked`信号。槽可以是Qt中的槽或 Python 响应的代码。`QtCore.QCoreApplication`包含了主事件循环。它处理并分解所有时间。`instance()`方法给出了其当前实例。注意`QtCore.QCoreApplication`随`QtGui.QApplication`创建。点击信号被连接到`quit()`方法，其将终止应用程序。在两个对象间的交互完成了。发送者和接收者。发送者是按钮，接收者是应用程序对象。

![Quit button程序的截图](http://zetcode.com/img/gui/pyqt4/quitbutton.png "Quit button")

图片：Quit button

## 消息盒子

默认如果我们点击标题栏上的X，`QtGui.QWidget`将会关闭。有事我们想修改这个默认的动作。例如，如果我们在一个编辑器中打开了一个文件，这个文件我们做了一些修改。我们要显示一个消息盒子来确认这一动作。

{% highlight python %}

#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PyQt4 tutorial 

This program shows a confirmation 
message box when we click on the close
button of the application window. 

author: Jan Bodnar
website: zetcode.com 
last edited: October 2011
"""

import sys
from PyQt4 import QtGui


class Example(QtGui.QWidget):
    
    def __init__(self):
    	super(Example, self).__init__()
    	
    	self.initUI()
        
	def initUI(self):               
	    
	    self.setGeometry(300, 300, 250, 150)        
	    self.setWindowTitle('Message box')    
	    self.show()
	    
	def closeEvent(self, event):
	    
	    reply = QtGui.QMessageBox.question(self, 'Message',
        	"Are you sure to quit?", QtGui.QMessageBox.Yes | 
        	QtGui.QMessageBox.No, QtGui.QMessageBox.No)
		
    	if reply == QtGui.QMessageBox.Yes:
        	event.accept()
    	else:
        	event.ignore()        
    	
    	
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()

{% endhighlight %}

如果我们关闭了`QtGui.QWidget`，`QtGui.QCloseEvent`将生成。为了修改部件的行为，我们需要重新实现`closeEvent()`事件处理器。

{% highlight python %}

reply = QtGui.QMessageBox.question(self, 'Message',
	"Are you sure to quit?", QtGui.QMessageBox.Yes | 
	QtGui.QMessageBox.No, QtGui.QMessageBox.No)

{% endhighlight %}

我们显示了带有两个按钮的消息盒子。Yes 和 No。第一个字符串出现在标题栏。第二个字符串作为消息文本。第三个参数指定了出现在对话框里的按钮组合。最后一个参数是默认按钮。它是拥有键盘焦点的那个按钮。返回值保存在`reply`变量中。

{% highlight python %}

if reply == QtGui.QMessageBox.Yes:
	event.accept()
else:
    event.ignore()

{% endhighlight %}

这里我们测试了返回值。如果我们点击 Yes，我们接受了这个事件，同时关闭部件并终止应用程序。否则，我们将忽略关闭事件。

![Message box](http://zetcode.com/img/gui/pyqt4/messagebox.png "Message box")

图片：Message box

## 居中显示窗口

下面的脚本展示如何在屏幕上居中显示窗口

{% highlight python %}

#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PyQt4 tutorial 

This program centers a window 
on the screen. 

author: Jan Bodnar
website: zetcode.com 
last edited: October 2011
"""

import sys
from PyQt4 import QtGui


class Example(QtGui.QWidget):
    
    def __init__(self):
    	super(Example, self).__init__()
    	
    	self.initUI()
        
    def initUI(self):               
	    
    	self.resize(250, 150)
    	self.center()
        
	    self.setWindowTitle('Center')    
    	self.show()
        
	def center(self):
    	
    	qr = self.frameGeometry()
    	cp = QtGui.QDesktopWidget().availableGeometry().center()
    	qr.moveCenter(cp)
    	self.move(qr.topLeft())
        
    	
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()

{% endhighlight %}

在屏幕上居中显示窗口

{% highlight python %}

self.center()

{% endhighlight %}

居中显示窗口的代码在`center()`方法中

{% highlight python %}

qr = self.frameGeometry()

{% endhighlight %}

我们将得到一个矩形来指定主窗口的几何大小。它包括了任何一个窗口框架。

{% highlight python %}

cp = QtGui.QDesktopWidget().availableGeometry().center()

{% endhighlight %}

我们计算出显示器的分辨率，然后得到屏幕的中心点。

{% highlight python %}

qr.moveCenter(cp)

{% endhighlight %}

我们的矩形已经有了宽度和高度。现在我们将矩形的中心设置在屏幕的中心。矩形的大小不会改变。

{% highlight python %}

self.move(qr.topLeft())

{% endhighlight %}

我们将应用程序窗口的左上角移动到`qr`矩形的左上角，因此窗口显示在屏幕的中间。

这部分教程讲解了一些 PyQt4 的基础知识。
