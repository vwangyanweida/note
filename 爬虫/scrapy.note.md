## 1. scrapy 架构图

![scrapy 架构图](http://scrapy-chs.readthedocs.io/zh_CN/0.24/_images/scrapy_architecture.png)

- Scrapy Engine: 事件驱动的事件管理类
> 负责组件之间数据的流转，当某个动作发生时触发事件。

- Scheduler：
> 接收requests， 并把它们入队，以便后续的调度。

- Downloader:
> 负责抓去网页，并传给引擎，之后抓取结果将传给spider

- Spiders：
> 用户编写的可定制化的部分，负责解析response，产生items和URL

- Item Pipeline：
> 负责处理item,典型的用途：清洗、验证、持久化

- Downloader middlewares：
> 位于引擎和下载器之间的一个钩子，处理传送到下载器的requests和传送到引擎的response（若需要在Requests到达Downloader之前或者是response到达spiders之前做一些预处理，可以使用该中间件来完成）

- Spider Middlewares：
> 位于引擎和抓取器之间的一个钩子，处理抓取器的输入和输出（在spiders产生的items到达Item Pipeline之前做一些预处理或response到达spider之前做一些处理)

**注意**：

1.  Middlewares 都是位于引擎和spider或者Downloader之间，各个结构都要通过引擎的事件驱动来触发操作，主程序是引擎的那个线程。其他各个部分之间不直接交互，全部靠引擎来传递事件和消息。

2. Spiders之中用到了yield，说明引擎会使用异步来处理各个请求。


## 2. Scrapy 数据流

1.  引擎打开一个网站(open a domain),找到处理该网站的spider，并向该spider请求第一个要爬取的url(s);

2. 引擎从spider中获取到第一个要爬取的url并在调度器(scheduler)以requests调度；

3. 引擎向调度器请求下一个要爬取的url；

4. 调度器返回下一个要爬取的url给引擎，引擎将url通过下载器中间件(请求requests方向)转发给下载器(Downloader);

5. 一旦页面下载完毕，下载器生成一个该页面的responses，并将其通过下载器中间件(返回responses方向)发送给引擎；

6. 引擎从下载器中接收到responses并通过spider中间件(输入方向)发送给spider处理；

7. spider处理responses并返回爬取到的Item及(跟进的)新的resquests给引擎

8. 引擎将(spider返回的)爬取到的Item给Item Pipeline，将(spider返回的)requests给调度器；
9. (从第二部)重复直到(调度器中没有更多的request)引擎关闭该网站

## 3. Spiders
1. 处理网页信息的方法：必须继承与scrapy.Spider,且定义三个属性、方法。

	1. 	name：爬虫名字，必须唯一
	
	2. 	start_urls：起始url
	
	3. 	parse：解析页面的方法,默认的 request 得到 response 之后会调用这个回调函数。也可以生成Request时手工指定回调函数。
	
	可选的属性：
	
	1. allowed_domains：应该是检测域名,爬取的网站必须符合域名。

2. 我们需要在这里对页面进行解析，返回两种结果（需要进一步 crawl 的链接和需要保存的数据）。Requests指定回调函数。返回的都用yield返回。

3. spider 爬取的循环类似下文

	1. 以初始的URL初始化Request，并设置回调函数。 当该request下载完毕并返回时，将生成response，并作为参数传给该回调函数。
	
	2. spider中初始的request是通过调用 start\_requests() 来获取的。 start\_requests() 读取 start_urls 中的URL， 并以 parse 为回调函数生成 Request 。
		
		该方法必须返回一个可迭代对象(iterable)。该对象包含了spider用于爬取的第一个Request。
		当spider启动爬取并且未制定URL时，该方法被调用。 当指定了URL时，make_requests_from_url() 将被调用来创建Request对象。 该方法仅仅会被Scrapy调用一次，因此您可以将其实现为生成器。
		
		该方法的默认实现是使用 start_urls 的url生成Request。
		
		如果您想要修改最初爬取某个网站的Request对象，您可以重写(override)该方法。 例如，如果您需要在启动时以POST登录某个网站，你可以这么写:

			class MySpider(scrapy.Spider):
			    name = 'myspider'
			
			    def start_requests(self):
			        return [scrapy.FormRequest("http://www.example.com/login",
			                                   formdata={'user': 'john', 'pass': 'secret'},
			                                   callback=self.logged_in)]
			
			    def logged_in(self, response):
			        # here you would extract links to follow and return Requests for
			        # each of them, with another callback
			        pass
	
	3. 在回调函数内分析返回的(网页)内容，返回 Item 对象或者 Request 或者一个包括二者的可迭代容器。 返回的Request对象之后会经过Scrapy处理，下载相应的内容，并调用设置的callback函数(函数可相同)。
	
	4. 在回调函数内，您可以使用 选择器(Selectors) (您也可以使用BeautifulSoup, lxml 或者您想用的任何解析器) 来分析网页内容，并根据分析的数据生成item。
	
	5. 最后，由spider返回的item将被存到数据库(由某些 Item Pipeline 处理)或使用 Feed exports 存入到文件中。

4. spider 可以接受参数，在执行crawl 时添加参加 -a可以传递参数，spider会在构造器里获取参数。
	
		scrapy crawl myspider -a category=eletronics
		
		class MySpider(Spider):
			name = "myspider"
			def __init__(self, category=None, *args, **kwargs):
				pass
	

5. spider 种类:
详细内容见：[spider详情](http://scrapy-chs.readthedocs.io/zh_CN/latest/topics/spiders.html#id2)

	**1. Spider**：class scrapy.spider.Spider

	> Spider是最简单的spider，所有其他Spider的基类:
	
	**属性**
	
	> name
	
	> allowed_domains
	
	> start_urls
	
	> custons_settings
	
	>crawler
	
	>settings

	>from_crawler
	
	>start_requests: 
	
		如果您想要修改最初爬取某个网站的Request对象，您可以重写(override)该方法
		def start_requests(self):
    		return [scrapy.FormRequest("http://www.example.com/login",
                               formdata={'user': 'john', 'pass': 'secret'},
                               callback=self.logged_in)]


	>make_requests_from_url

	>parse

	>log

	>closed
	

	**2.CrawlSpider**: class scrapy.contrib.spider.CrawlSpider
	>爬取一般网站常用的spider。其定义了一些规则(rule)来提供跟进link的方便的机制。

	**属性**：
	> **rules**: 一个包含一个(或多个) Rule 对象的集合(list)。 每个 Rule 对爬取网站的动作定义了特定表现。 Rule对象在下边会介绍。 如果多个rule匹配了相同的链接，则根据他们在本属性中被定义的顺序，第一个会被使用。

	> parse_start_url
	
	**爬取规则（Crawling rules）**

	>class scrapy.contrib.spiders.Rule(link_extractor, callback=None, cb_kwargs=None, follow=None, process_links=None, process_request=None)

	**example**

		import scrapy
		from scrapy.contrib.spiders import CrawlSpider, Rule
		from scrapy.contrib.linkextractors import LinkExtractor

		class MySpider(CrawlSpider):
		    name = 'example.com'
		    allowed_domains = ['example.com']
		    start_urls = ['http://www.example.com']
		
		    rules = (
		        # 提取匹配 'category.php' (但不匹配 'subsection.php') 的链接并跟进链接(没有callback意味着follow默认为True)
		        Rule(LinkExtractor(allow=('category\.php', ), deny=('subsection\.php', ))),
		
		        # 提取匹配 'item.php' 的链接并使用spider的parse_item方法进行分析
		        Rule(LinkExtractor(allow=('item\.php', )), callback='parse_item'),
		    )
		
		    def parse_item(self, response):
		        self.log('Hi, this is an item page! %s' % response.url)
		
		        item = scrapy.Item()
		        item['id'] = response.xpath('//td[@id="item_id"]/text()').re(r'ID: (\d+)')
		        item['name'] = response.xpath('//td[@id="item_name"]/text()').extract()
		        item['description'] = response.xpath('//td[@id="item_description"]/text()').extract()
		        return item

	**3.XMLFeedSpider**: class scrapy.contrib.spiders.XMLFeedSpider
	>XMLFeedSpider被设计用于通过迭代各个节点来分析XML源(XML feed)。

	**4.CSVFeedSpider**: class scrapy.contrib.spiders.CSVFeedSpider
	>该spider除了其按行遍历而不是节点之外其他和XMLFeedSpider十分类似。 而其在每次迭代时调用的是 parse_row() 。

	**5.SitemapSpider**: class scrapy.contrib.spiders.SitemapSpider
	>SitemapSpider使您爬取网站时可以通过 Sitemaps 来发现爬取的URL。

	>其支持嵌套的sitemap，并能从 robots.txt 中获取sitemap的url。
	

## 3. Item
	就像是ORM一样，保存从网页提取的信息结构。

1. Selectors 选择器

	>从网页中提取数据有很多方法。Scrapy使用了一种基于 XPath 和 CSS 表达式机制: Scrapy Selectors

2. Selector有四个基本的方法

	- xpath(): 传入xpath表达式，返回该表达式所对应的所有节点的selector list列表 。
	- css(): 传入CSS表达式，返回该表达式所对应的所有节点的selector list列表.
	- extract(): 序列化该节点为unicode字符串并返回list。
	- re(): 根据传入的正则表达式对数据进行提取，返回unicode字符串list列表。

3. 提取item每个 response.xpath() 调用返回selector组成的list，因此我们可以拼接更多的 .xpath() 来进一步获取某个节点

4. Item Loaders
	
## 4. 命令行工具

- feed输出：scrapy crawl name -o out.json

小型项目输出json，大型项目自己写Item Pipeline 输出到数据库

命令执行流程：

>Scrapy为Spider的 start_urls 属性中的每个URL创建了 scrapy.Request 对象，并将 parse 方法作为回调函数(callback)赋值给了Request。

>Request对象经过调度，执行生成 scrapy.http.Response 对象并送回给spider parse() 方法。

- 无参数scrapy，输出scrapy支持的命令和提示
> scrapy

- 创建项目
> scrapy startproject myporject

- 控制项目

	1. 全局命令：
	
			startproject
			settings
			runspider
			shell
			fetch
			view
			version

	2. 项目命令：

			crawl
			check
			list
			edit
			parse
			genspider
			deploy
			bench

- 命令的执行

	1. scrapy是一个py文件，它加到了系统路径所以可以直接执行。通过which scrapy可以找到文件路径。
	
	2. 入口是scrapy/cmdline.py的execute方法。
			
			from scrapy import cmdline
			cmdline.execute("scrapy crawl fang".split())

## 5 配置文件
两个中间件和Item pipeline都是通过在setting里直接指到那个类的。