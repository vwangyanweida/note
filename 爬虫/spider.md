## 爬虫原理与数据抓取
### 1. 通用爬虫和聚焦爬虫

1. 通用爬虫
	> 通用网络爬虫 是 捜索引擎抓取系统（Baidu、Google、Yahoo等）的重要组成部分。主要目的是将互联网上的网页下载到本地，形成一个互联网内容的镜像备份
	
	1. 通用搜索引擎（Search Engine）工作原理
		1. 抓去网页
			1. 首先选取一部分的种子URL，将这些URL放入待抓取URL队列；
			2. 取出待抓取URL，解析DNS得到主机的IP，并将URL对应的网页下载下来，存储进已下载网页库中，并且将这些URL放进已抓取URL队列。
			3. 分析已抓取URL队列中的URL，分析其中的其他URL，并且将URL放入待抓取URL队列，从而进入下一个循环....
		
		2. 数据存储
			搜索引擎通过爬虫爬取到的网页，将数据存入原始页面数据库。其中的页面数据与用户浏览器得到的HTML是完全一样的。

			搜索引擎蜘蛛在抓取页面时，也做一定的重复内容检测，一旦遇到访问权重很低的网站上有大量抄袭、采集或者复制的内容，很可能就不再爬行。

		3. 预处理
			- 提取文字
			- 中文分词
			- 消除噪音（比如版权声明文字、导航条、广告等……）
			- 索引处理
			- 链接关系计算
			- 特殊文件处理
			- 搜索引擎还不能处理图片、视频、Flash 这类非文字内容，也不能执行脚本和程序。
			
		4. 提供检索服务，网站排名
			根据页面的PageRank值（链接的访问量排名）进行网站排名 or money购买排名
	
	2. 局限性
		1. 不能专注某一个方面

2. 聚焦爬虫
	接来下讲的都是聚焦爬虫

### 2. HTTP和HTTPS

1. 协议：

	1. http 超文本传输协议
	2. https 在http协议下加入SSL层
	3. ssl  安全套接层，在传输层对网络链接进行加密，保障在Internet上数据传输的安全。
	4. HTTP 端口 80， HTTPS 端口443
	
2. URL基本格式: scheme://host[:port#]/path/…/[?query-string][#anchor]

	1. scheme：协议(例如：http, https, ftp)
	2. host：服务器的IP地址或者域名
	3. port#：服务器的端口（如果是走协议默认端口，缺省端口80）
	4. path：访问资源的路径
	5. query-string：参数，发送给http服务器的数据
	6. anchor：锚（跳转到网页的指定锚点位置）

3. HTTP请求 


	典型的HtTP请求实例
	
		GET https://www.baidu.com/ HTTP/1.1
		Host: www.baidu.com
		Connection: keep-alive
		Upgrade-Insecure-Requests: 1
		User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36
		Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
		Referer: http://www.baidu.com/
		Accept-Encoding: gzip, deflate, sdch, br
		Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
		Cookie: BAIDUID=04E4001F34EA74AD4601512DD3C41A7B:FG=1; BIDUPSID=04E4001F34EA74AD4601512DD3C41A7B; PSTM=1470329258; MCITY=-343%3A340%3A; BDUSS=nF0MVFiMTVLcUh-Q2MxQ0M3STZGQUZ4N2hBa1FFRkIzUDI3QlBCZjg5cFdOd1pZQVFBQUFBJCQAAAAAAAAAAAEAAADpLvgG0KGyvLrcyfrG-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFaq3ldWqt5XN; H_PS_PSSID=1447_18240_21105_21386_21454_21409_21554; BD_UPN=12314753; sug=3; sugstore=0; ORIGIN=0; bdime=0; H_PS_645EC=7e2ad3QHl181NSPbFbd7PRUCE1LlufzxrcFmwYin0E6b%2BW8bbTMKHZbDP0g; BDSVRTM=0

4. HTTP的方法：
	1. 所有方法：
		1. GET	请求指定的页面信息，并返回实体主体。
		2. HEAD	类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
		3. POST	向指定资源提交数据进行处理请求（例如提交表单或者上传文件），数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
		4. PUT	从客户端向服务器传送的数据取代指定的文档的内容。
		5. DELETE	请求服务器删除指定的页面。
		6. CONNECT	HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
		7. OPTIONS	允许客户端查看服务器的性能。
		8. TRACE	回显服务器收到的请求，主要用于测试或诊断。

	2. 主要使用GET和POST方法
		1. GET：参数显示在浏览器上，是url的一部分
		2. POST：请求参数在请求体中，消息长度没有限制且隐式发送。 当提交比较大的数据，请求参数包含在"Content-Type"消息头里，指明该消息的媒体类型和编码。

5. HTTP请求的头部

	pass

6. 服务端HTTP的响应
	
	HTTP响应也由四个部分组成，分别是： 状态行、消息报头、空行、响应正文

	1. 常用的响应报头
		1. Cache-Control：must-revalidate， no-cache，private：服务端不希望客户端缓存资源，下次请求资源时，必须重新请求服务器。
			1. Cache-Control是响应头中很重要的信息，当客户端请求头中包含Cache-Control:max-age=0请求，明确表示不会缓存服务器资源时,Cache-Control作为作为回应信息，通常会返回no-cache，意思就是说，"那就不缓存呗"。

			2. 当客户端在请求头中没有包含Cache-Control时，服务端往往会定,不同的资源不同的缓存策略，比如说oschina在缓存图片资源的策略就是Cache-Control：max-age=86400,这个意思是，从当前时间开始，在86400秒的时间内，客户端可以直接从缓存副本中读取资源，而不需要向服务器请求。

	2. Connection：keep-alive
	
		这个字段作为回应客户端的Connection：keep-alive，告诉客户端服务器的tcp连接也是一个长连接，客户端可以继续使用这个tcp连接发送http请求。

	3. 响应状态码
		1. 100~199：表示服务器成功接收部分请求，要求客户端继续提交其余请求才能完成整个处理过程。

		2. 200~299：表示服务器成功接收请求并已完成整个处理过程。常用200（OK 请求成功）。

		3. 300~399：为完成请求，客户需进一步细化请求。例如：请求的资源已经移动一个新地址、常用302（所请求的页面已经临时转移至新的url）、307和304（使用缓存资源）。
	
		4. 400~499：客户端的请求有错误，常用404（服务器无法找到被请求的页面）、403（服务器拒绝访问，权限不够）。		
		
		5. 500~599：服务器端出现错误，常用500（请求未完成。服务器遇到不可预知的情况）。

	4. Cookie 和 Session：

		服务器和客户端的交互仅限于请求/响应过程，结束之后便断开，在下一次请求时，服务器会认为新的客户端。
		
		为了维护他们之间的链接，让服务器知道这是前一个用户发送的请求，必须在一个地方保存客户端的信息。
		
		Cookie：通过在 客户端 记录的信息确定用户的身份。
		
		Session：通过在 服务器端 记录的信息确定用户的身份。

### 3. Fiddler
Fiddler是一款强大Web调试工具，它能记录所有客户端和服务器的HTTP请求。 Fiddler启动的时候，默认IE的代理设为了127.0.0.1:8888，而其他浏览器是需要手动设置。

略

### 4. python 库

	urllib， urllib2， resquest：request可以满足所有需求

### 5. 非结构化数据处理

	1. 非结构化的数据处理

		1. 文本、电话号码、邮箱地址
			正则表达式

		2. HTML 文件
			正则表达式
			XPath
			CSS选择器

	2. 结构化的数据处理

		1. JSON 文件
			JSON Path
			转化成Python类型进行操作（json类）
		
		2. XML 文件
			转化成Python类型（xmltodict）
			XPath
			CSS选择器
			正则表达式
	3. 类库
		1. re
		2. XPath、lxml
		3. beautifulSoup4
		4. JsonPath
		

### 6. 动态HTML处理和机器图像识别

1. Selenium和PhantomJS
	>Selenuum配合PhantomJS 功能很强大
	
	1. 定位UI元素
	
		1. find\_element\_by\_id
		2. find\_elements\_by\_name
		3. find\_elements\_by\_xpath
		4. find\_elements\_by\_link\_text
		5. find\_elements\_by\_partial\_link\_text
		6. find\_elements\_by\_tag\_name
		7. find\_elements\_by\_class\_name
		8. find\_elements\_by\_css\_selector

	2. 鼠标动作链
		>有些时候，我们需要再页面上模拟一些鼠标操作，比如双击、右击、拖拽甚至按住不动等，我们可以通过导入 ActionChains 类来做到：
	
2. Tesseract