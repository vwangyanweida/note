### 1. 模块
-	QtCore
-	QtGui
-	QtWidgets
-	QtMultimedia
-	QtBluetooth
-	QtNetwork
-	QtPositioning
-	Enginio
-	QtWebSockets
-	QtWebKit
-	QtWebKitWidgets
-	QtXml
-	QtSvg
-	QtSql
-	QtTest


>QtCore:包含了核心的非GUI功能。此模块用于处理时间、文件和目录、各种数据类型、流、URL、MIME类型、线程或进程。

>QtGui包含类窗口系统集成、事件处理、二维图形、基本成像、字体和文本。

>qtwidgets模块包含创造经典桌面风格的用户界面提供了一套UI元素的类。

>QtMultimedia包含的类来处理多媒体内容和API来访问相机和收音机的功能。

>Qtbluetooth模块包含类的扫描设备和连接并与他们互动。描述模块包含了网络编程的类。这些类便于TCP和IP和UDP客户端和服务器的编码，使网络编程更容易和更便携。

>Qtpositioning包含类的利用各种可能的来源，确定位置，包括卫星、Wi-Fi、或一个文本文件。

>Enginio模块实现了客户端库访问Qt云服务托管的应用程序运行时。

>Qtwebsockets模块包含实现WebSocket协议类。

>QtWebKit包含一个基于Webkit2图书馆Web浏览器实现类。

>Qtwebkitwidgets包含的类的基础webkit1一用于qtwidgets应用Web浏览器的实现。

>QtXml包含与XML文件的类。这个模块为SAX和DOM API提供了实现。

>QtSvg模块提供了显示SVG文件内容的类。可伸缩矢量图形（SVG）是一种描述二维图形和图形应用的语言。

>QtSql模块提供操作数据库的类。

>QtTest包含的功能，使pyqt5应用程序的单元测试

### 2. PyQt5不兼容PyQt4

- python模块已经重组。一些模块已经删除（qtscript），有的被分割成子模块（QtGui，QtWebKit）。
- 新的模块作了详细的介绍，包括qtbluetooth，qtpositioning，或enginio。
- pyqt5只支持新型的信号和槽handlig。电话signal()或slot()不再支持。
- pyqt5不支持Qt的API被标记为过时或陈旧的任何部分在QT V5.0。

### 3. qtcreator4快速入门

####1.窗口部件
>窗口部件就是没有嵌入其他部件的部件，一般有边框和标题。QMainWindow和QDialog是最一般的窗口。

1. 基础窗口部件：

	1. 窗口、子部件以及窗口类型

		- 默认基类： QMainWindow、QWidget、QDialog 三类。
		- QWidget 有两个参数：父类、窗口标志。窗口标志可以是窗口类型和窗口标志的或。

	2. 对话框
		- 模态和非模态对话框：
			> 模态对话框就是没有关闭它之前，不能在于同一个应用程序的其他窗口进行交互，比如新建项目时的对话框。非模态对话框，既可以与它交互，也可以与统一程序中的其他程序交互。
			
			- 如果想要是一个对话框成为模态对话框，只需调用他的exec()函数，而要使它成为非模态对话框，使用new操作来创建，然后show()函数创建。使用show函数和可以建立模态对话框，只需哟啊在前面使用setModal()函数即可。
				
			- **不用new创建的对象实在栈空间，new在堆空间，程序离开函数会清除函数栈的数据，new的堆数据必须手动del。子控件不用自己del，父控件会自动清除子控件空间。**

			- exec会阻塞住程序的后续执行，show函数会妈马上将控制权转交给调用者。与setModel相似有setWindowModao函数，可以设置阻塞大的程度参数。
			 

		- 多窗口切换
		
		- 标准对话框

	3. 其他窗口部件
		- QFrame类族
		- 按钮部件
		- 行编辑器
		- 数值设定框
		- 滑块部件

2. 布局管理
	1. 布局管理系统
	2. 设置伙伴
	3. 设置tab
	
3. 应用程序主窗口

	qt designer 中所有控件对应的需求
		

4. 事件系统

5. Qt对象模型与容器类

6. 界面外观

7. http与数据库