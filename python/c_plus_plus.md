### 构造函数：

1. explict
	1. C++中的explicit关键字只能用于修饰只有一个参数的类构造函数, 它的作用是表明该构造函数是显示的, 而非隐式的, 跟它相对应的另一个关键字是implicit, 意思是隐藏的,类构造函数默认情况下即声明为implicit(隐式).

	2. explicit关键字只对有一个参数的类构造函数有效, 如果类构造函数参数大于或等于两个时, 是不会产生隐式转换的, 所以explicit关键字也就无效了
	3. 也有一个例外, 就是当除了第一个参数以外的其他参数都有默认值的时候, explicit关键字依然有效, 此时, 当调用构造函数时只传入一个参数, 等效于只有一个参数的类构造函数

2. c++中字符串、字符串指针、const 字符串指针的区别


3. 类对象在内存中的存储位置：
	1.	一种是在栈上创建，形式如下： CSomeClass someObject;
	2. 一种是在堆上创建(动态分配)，形式如下： CSomeClass *pSomeObject = new CSomeClass();

4. c++ 类对象在内存的位置和虚指针虚表的详解：

	1. [虚表，内存位置](https://www.cnblogs.com/jerry19880126/p/3616999.html)

	2. 
		- 类的普通成员函数以及静态成员函数不占内存

		类的成员函数实际上与普通的全局函数类似，只不过在编译的时候会在成员函数上加一个指针（this）形参，（这非常重要）比如上面的方法可以理解为Show（&a）;这   样调用Show的时候就传入了a的地址。成员函数的地址是全局已知的，对象内存无需保存，对象也并不知道它各个函数的地址（这里理解是其他的途径先调用该函数，再把对象传入函数的形参中）

		- 有虚函数的类中会有一个维护虚函数表的指针，这样会占4个字节，64位编译器是可能是8个字节。
		
		- 类的属性（成员变量），在实例化一个对象时就为这些数据成员分配了内存，而且他们是相互独立的。
		
		- 静态成员函数与一般成员函数的区别是没有this指针，因此不能访问非静态的数据成员。
		
		- 程序中的所有函数位于代码区（C语言内存分配中有代码区用于存放CPU执行的机器指令与代码等，其中还有data数据区，BSS区，堆栈）
		
		- 静态数据成员不属于某个对象（未初始化的存储在bss区，初始化的存储在data区）
		
		- 成员变量所占大小存在内存对齐机制（在Struct中也是一样的）
		
	         比如上面的a,b,c分别是int，char，double类型，sizeof（int）+sizeof（char）<sizeof（double）。这里简单说一下对齐机制，首先第一个成员变量是int，所以先申请4个字节的大小，接下来是一个char，一个字节紧跟在int后面，然后double来了，它以为内存是按照自己的整数倍来分配的（每个关键字都是这么认为的），所以会直接空出8个字节（也就是char后面空出来3个字节），再进行添加。最后要以double类型为基础单位来占用内存（必须是double大小的整数倍）。
		
		8. 空类占一个字节
		
	          空类也需要实例化，实例化必须要在内存中分配一块地址，编译器一般会默认添加一个字节	
		
			