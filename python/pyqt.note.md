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

### 3. qtcreator窗口部件
>窗口部件就是没有嵌入其他部件的部件，一般有边框和标题。QMainWindow和QDialog是最一般的窗口。

1. 基础窗口部件：

	1. 窗口、子部件以及窗口类型

		- 默认基类： QMainWindow、QWidget、QDialog 三类。
		- QWidget 有两个参数：父类、窗口标志。窗口标志可以是窗口类型和窗口标志的或。
			**窗口标志可以调节很多东西，比如只显示一个关闭x，就是设置这个标志，所有窗口相关设置都在这里。**
				
				Widget: 是一个窗口或部件，有父窗口就是部件，没有就是窗口
				Window: 是一个窗口，有窗口边框和标题
				Dialog: 是一个对话框窗口
				Sheet: 是一个窗口或部件Macintosh表单
				Drawer: 是一个窗口或部件Macintosh抽屉
				Popup: 是一个弹出式顶层窗口
				Tool: 是一个工具窗口
				ToolTip: 是一个提示窗口，没有标题栏和窗口边框
				SplashScreen: 是一个欢迎窗口，是QSplashScreen构造函数的默认值
				Desktop: 是一个桌面窗口或部件
				SubWindow: 是一个子窗口
				ForeignWindow: 
				CoverWindow:
				WindowType_Mask:
				MSWindowsFixedSizeDialogHint:
				MSWindowsOwnDC:
				BypassWindowManagerHint:
				X11BypassWindowManagerHint:
				FramelessWindowHint: 创建一个无标题、无边框的窗口
				WindowTitleHint: 为窗口修饰一个标题栏
				WindowSystemMenuHint: 为窗口修饰一个窗口菜单系统
				WindowMinimizeButtonHint: 为窗口添加最小化按钮
				WindowMaximizeButtonHint: 为窗口添加最大化按钮
				WindowMinMaxButtonsHint: 为窗口添加最大化和最小化按钮
				WindowContextHelpButtonHint:
				WindowShadeButtonHint:
				WindowStaysOnTopHint:
				WindowTransparentForInput:
				WindowOverridesSystemGestures:
				WindowDoesNotAcceptFocus:
				MaximizeUsingFullscreenGeometryHint:
				CustomizeWindowHint: 关闭默认窗口标题提示
				WindowStaysOnBottomHint:
				WindowCloseButtonHint:
				MacWindowToolBarButtonHint:
				BypassGraphicsProxyWidget:
				NoDropShadowWindowHint:
				WindowFullscreenButtonHint:


	2. 对话框
		- 模态和非模态对话框：
			> 模态对话框就是没有关闭它之前，不能在于同一个应用程序的其他窗口进行交互，比如新建项目时的对话框。非模态对话框，既可以与它交互，也可以与统一程序中的其他程序交互。
			
			- 如果想要是一个对话框成为模态对话框，只需调用他的exec()函数，而要使它成为非模态对话框，使用new操作来创建，然后show()函数创建。使用show函数和可以建立模态对话框，只需哟啊在前面使用setModal()函数即可。
				
			- **不用new创建的对象实在栈空间，new在堆空间，程序离开函数会清除函数栈的数据，new的堆数据必须手动del。子控件不用自己del，父控件会自动清除子控件空间。**

			- exec会阻塞住程序的后续执行，show函数会妈马上将控制权转交给调用者。与setModel相似有setWindowModao函数，可以设置阻塞大的程度参数。
			 

		- 多窗口切换
		
		- 标准对话框

2. qt designer 中所有控件对应的需求

	- 按钮组
	 
	 - Push Button：按钮
	 -	Tool Buuton：工具按钮
	 - 	Radio button：单选框框	
	 - 	Check Box：多选框
	 -	Command Link Button：命令连接按钮
	 -	Button Box：按钮盒

	- 输入控件组
	
	 	- Combo Box ：组合框
		- Font Combo Box:字体组合框
		- Line Edit：行编辑
		- Text Edit：文本编辑
		- Plain Text Edit:纯文本编辑
		- Spin Box：数字显示框
		- Double Spin Box：双自旋盒
		- Time Edit：时间编辑
		
		- **注意的类**：
			- QDateTime 获取系统时间
			- Qtime类：定时器，可以定时发送timeout()信号，触发slottimedone槽

	- 显示控件组
		- Label：标签
		- Text Browser：文本浏览器
		- Graphics View：图形视图
		- Calendar：日历
		- LCD Number 液晶数字
		- Progress Bar 进度条
		- Horizontal Line：水平线
		- Vertical Line :垂直线
		- QDeclarative View：向QML暴露数据
		- QWebView: Web视图
		
		- **注意的类**：
			- Text Browser对应于QtextBroser类。QTextBrowser继承于QTextEdit类且是只读的，具有多个属性
	 
	- 空间间隔组
	 	- Horizontal Spacer：水平间隔
	 	- Vertical Spacer：垂直间隔 用来填满空间
	 	
	- 布局管理组
		- Vertical Layout：垂直布局
		- Horizontal Layout：横向布局
		- Grid Layout：网格布局
		- From Layout：表格布局
		
	- 容器组
		- Group Box:组框
		- Scroll Area：滚动区域
		- Tool Box：工具箱
		- Tab Widget：标签小部件
		- Stacked Widget：堆叠部件
		- Frame：帧
		- Widget：小部件
		- MdiArea：MDI区域
		- Dock Widget：停靠窗体部件
		- QAx Wdiget:封装Flash的Activex控件
		
		- **注意的类**
			- Widget对应的类是QWidget。
				> QWidget是所有的QT Gui界面类的基类，它接受鼠标、键盘及其他窗口事件，并在显示器上绘制自己。
				- 构造函数： QWidget（Qwidget *parent=0， QT::WindosFlags f=0）
					- parent：父窗口，没指定表明新建一个窗口，否则是parent的子窗口部件。
					- f：窗口标识：定义了窗口部件类型的窗口类型和窗口提示。窗口类型指定窗口系统属性只能一个。出啊口提示定义顶层窗口的外观，可以多个。
				

	- 项目视图组
		- List View：清单视图
		- Tree View：树视图
		- Table View：表视图
		- Column View：列视图
		
		- **注意的类**
			
	- 项目控件组
		- List Widget：清单控件
		- Tree Widget：树形控件
		- Table Widget：表控件


###4. 基本对话框
1. 标准文件对话框
	1. QFileDialog：
		1. getOpenFileName返回文件名或空串
		2. 静态函数
		3. 参数：
		
				(QWidget *parent=0,	父窗口
				const QString & caption=QString(), 标题名
				cosnt QString & dir=QString（），默认目录或文件名
				const QString & filter=QString()， 过滤,(类型或其他）
				QString * selectedFilter=0，用户选择的过滤器通过此参数返回
				Options options=0  选择显示文件名的格式，默认同时选择目录和文件名
				)
2. 标准颜色对话框
	1. getColor()静态函数，返回选择的颜色值
	2. 参数
		
			（const QColor& initial=Qt::white, 默认白色
			  QWidget* parent=0）
	3. 选中颜色后，
		1. QColor c = QColorDialog::getColor(Qt::blue); 
		2. QFrame->setPalettr(QPalette(c)) 绘制颜色

3. 标准字体对话框
	1. getFont()是QFontDialog类的静态函数，返回选择的字体
	2. 参数：
	3. 
			QFont getFont(bool* ok,  用户是否ok
			QWidget* parent=0)		父窗口

4. 标准输入对话框
5. 消息对话框
6. 自定义消息框
7. 工具盒类
8. 进度条
	1. QProgressBar：任务完成情况
		1. 
	2. QProgressDialog：慢速过程
9. 调色板喝电子钟
10. 可扩展对话框
11. 不规则窗体
	> 利用setMask()为窗体设置遮罩，实现不规则窗体。设置遮罩后窗体尺寸不变，但是有的地方不可见。
	1.  
12. 程序启动画面
	1. Qt中提供QSplashScreen类实现了在程序启动过程中显示启动画面的功能。

###5. 主窗口 QMainWindow
1. 包含：菜单栏、工具栏、锚接部件、状态栏及中心部件
2. 菜单栏
	1. Action来表示菜单、工具按钮、键盘快捷方式等命令。
3. 状态栏

4. 工具栏
	1. 可以有多个，可以停靠在四个方向上
5. 锚接部件
	>作为一个容器使用，以包容其他窗口部件来实现某些功能。
6. 中心部件
	**QMainWindow具有自己的布局管理器，因此在QMainWindow窗口上设置布局管理器或者创建一个父窗口部件作为QMainWindow的布局管理器都是不允许的，但可以在中心部件上设置管理器**
7. 上下文菜单
	
	为了实现控制主窗口工具栏喝锚接器的显隐，QMainWindow提供了一个上下文菜单，可以通过单机右键激活，也可以通过QMainWindow::createPopupMenu()激活菜单。也可以重写createPopupMenu()函数实现自定上下文菜单。
		
###6. 文件

###7. 网络通信

###8. 模型和视图

	
### 碰到的问题
1. 原生qt里，是可以直接操作字符串的，显示，读取都可以。注意隐藏的模块要先显示才可以看到。