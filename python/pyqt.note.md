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

	- 其他
		- QTimer类提供了重复和单次触发信号的定时器。
			- 重复触发信号
			
					QTimer *timer =  new QTimer(this);
					connect(timer, SIGNAL(timeout()),this,SLOT(update()));
					timer->start(1000);		/ms
					start()之后，每隔一千毫秒就是1秒就会触发一次timeout信号

			- 单次触发
				>以通过设置setSingleShot(true)来让定时器只执行一次。也可以使用静态函数QTimer::singleShot()：
			
					QTimer::singleShot(200, this, SLOT(updateCaption()));



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
1. 读写文本文件：通常两种方法。
	1. 利用传统的QFile类方法：
		1. 这个主意如果打开文件路径是相对路径的话，相对路径的根目录是编译文件的根目录。

	2. 利用更为方便的QTextStream类方法
		1. 这个类可以操作QIODevice、QByteArray和QString
		
2. 读写二进制文件
	1. QDataStream 提供了将二进制文件串行化的功能。用于实现C++基本数据类型，如char、short、int、char*等的串行化。更复杂的串行化则是通过将数据类型分解为基本类型来完成。
	2. **Qt本身定义的类型直接输出文件中基本都是二进制的，读的时候注意读到相应的类型上面。** 
	
3. 目录操作与文件系统：QDir类具有存取目录结构和内容的能力，可以操作目录、存取目录或文件信息、操作底层文件系统，还可以存取Qt的资源文件。

	1. 文件大小和文件信息
		1. 使用"/"作为通用的目录分隔符和URL路径分隔符
		2. 可以使用相对路径或者绝对路径指向一个文件。isRelative()和isAbsolute()函数可以判断QDir()对象使用的是相对路径还是绝对路径。
		3. 讲一个相对路径转为换绝对路径，使用makeAbsolute()函数。
		4. 目录的路径可以使用path()来返回。setPath()函数设置新的路径。absolutePath()返回绝对路径。dirName()获得目录名，它通常返回绝对路径的最后一个元素，如果在当前目录则返回"."。
		5. cd()和cdUp()用来改变目录的路径。
		6. mkdir()创建目录
		7. rename()改变目录名
		8. remove函数删除文件，rmdir删除目录
		9. count统计总条目数，entryList返回目录下所有条目组成的字符串链表。
		10. exists()判断目录是否存在
		11. 目录属性通过使用isReadable()、isAbsolute()、isRelative()和isRoot()获取。
	
	2. 文件系统浏览实例
		1. 文件系统的浏览是目录操作的一个常用功能
	
4. 获取文件信息

5. 监视文件和目录变化

6. 删除

###7. 网络通信
1. pyqt的网络通信中，udp的传送和接收没有文档也看不到源代码，它们传送的格式一般为bytes，注意编解码，recevie datagram时，接收到的是一个三元素元组，注意解包。

2. **readyread()**是QIODevice的signal，QTcpSocket继承此类。QIODevice是所有输入输出设备的一个抽象类，其中定义了基本的接口，在Qt中，QTcpSocket也被看作一个QIODevice，readyRead()信号在有数据到来时发出。

3. Qt网络既可以操作udp，tcp等底层链接，也提供来了封装好的QNetworkRequest、QNetworkReply和QNetworkAccessManager这几个高层的类，他们提供更加简单和强大的借口。

### Qt 多线程
1. 多线程实现：继承QThread，重写run方法。
2. 多线程控制
	1. 互斥量： 通过QMutex或者QMutexLocker类实现
		1. QMutex类 
			1. lock()函数用于锁住互斥量，如果互斥量处于解锁状态，则当前线程立刻抓住他并锁定，否则阻塞到持有互斥量的线程解锁。unlock解锁。
			2. tryLock()函数，如果互斥量已被锁定，则立即返回。不会阻塞。
			
		2. QMutexLocker类：
			 > QMutexLocker用来简化互斥量的处理，它在构造函数中接收一个QMutex对象作为参数并将其锁定，在析构函数中解锁这个互斥量。
			 	
				class Key{
				public:
					Key(){key=0;}
					int creatKey(){QMutexLocker.locker(&mutex);  ++key; return key;}
					int value() const {QMutexLocker.locker(&mutex); return key;}
				private:
					int key;
					QMutex mutex;
				};

			
	2. 信号量
		1. 互斥锁只能锁定一次而信号量可以获取多次，它可以用来保护一定数量的同种资源。典型用例是保护生产者消费者之间共享的环形缓冲区。
		
	3. 等待与唤醒：QWaitCondition，允许线程在一定条件下唤醒其他线程。
		1. 
注：
1. 线程因为将因为调用printf()而持有一个控制I/O锁(lock)，多个线程同事调用printf函数在某些情况下将造成控制台输出阻塞，而是用Qt提供的qDebug()函数作为控制台输出则不会出现上述问题。
 

###8. 模型和视图

	
### 碰到的问题
1. 原生qt里，是可以直接操作字符串的，显示，读取都可以。注意隐藏的模块要先显示才可以看到。

2. Qt里的tr函数

	>函数tr()的原型是QObject::tr(),被它处理的字符串可以使用工具提取出来翻译成其他语言，也就是国际化使用。
	
	>如果想让你的程序实现国际化，那么就在用户所有可见的字符串处都使用QObject::tr()!但是我们在使用的过程中通常是使用tr()，而并非是 QObject::tr()，这是为什么呢？原来，tr()函数是定义在QObject里面的，所有使用了Q_OBJECT宏的类都自动具有tr()的函数。和connect函数一样，都是继承了QObject所以能够直接使用。


3. Qt中在按钮上显示字符'&'：
	1. 通常使用含字符'&'的字符串为按钮的显示名称，如果设置按钮的text为 "&Cancel",
即设置text, setText("&Cancel");或创建时 QPushButton *pushButton = new QPushButton (QObject::tr("&Cancel"));
Qt的编译器会将字符'&'后的'C'在显示时下方多一下划线，表明'C'为该按钮的快捷键，通过"Alt＋c"操作来实现对pushButton的点击。
	2. 有的时候，我们会有这种需求，想在按钮上或别的控件上以包括字符'&'为显示名如"Cancel&Exit"，这样字符串中的'&'需要对其进行转义，
通常我们会想到使用"/"来实现，试过发现不行，后来，直接使用'&'来转义没想到出现了想要的效果。即tr("Cancle&&Exit")。

4. Qtl里可能使用unicode编码，所以可以转换成其他编码来转换字符编码

5. QByteArray
	>QByteArray适合的两个主要情况是当您需要存储原始二进制数据，并且当内存保护至关重要时（例如，使用嵌入式Linux的Qt）
	
	1. 初始化QByteArray的方法
		1. 是const char *将其传递给其构造函数
		
				QByteArray ba("Hello")
		2. 是使用resize()设置数组的大小，并初始化每个字节的数据字节.

	2. QByteArray使用基于0的索引，就像C ++数组一样。 
		
		要访问特定索引位置的字节，可以使用operator[] ()在非 常量字节数组上，operator 返回一个可以在赋值左侧使用的字节的引用.

	3. 对于只读访问，替代语法是使用at(),at()可以比operator[]更快，因为它不会导致深层拷贝发生。