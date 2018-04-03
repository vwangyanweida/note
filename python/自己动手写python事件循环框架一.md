# 500 lines web craw 读书笔记


## 1. 阻塞socket 

	def fetch(url):
    sock = socket.socket()
    sock.connect(('xkcd.com', 80))
    request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(url)
    sock.send(request.encode('ascii'))
    response = b''
    chunk = sock.recv(4096)
    while chunk:
        response += chunk
        chunk = sock.recv(4096)

    # Page is now downloaded.
    links = parse_links(response)
    q.add(links)

这时socket上的accept、connect、recv等操作都是阻塞的，这时只能开线程和进程，会大量消耗系统资源。解决不了C10k问题

## 2. 非阻塞socket /Async
	
	sock = socket.socket()
	sock.setblocking(False)
	try:
	    sock.connect(('xkcd.com', 80))
	except BlockingIOError:
	    pass

----------

	request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(url)
	encoded = request.encode('ascii')
	
	while True:
	    try:
	        sock.send(encoded)
	        break  # Done.
	    except OSError as e:
	        pass
	
	print('sent')

这时socket的操作不是阻塞的，但是会立即返回，如果socket的操作没有成功，会立即返回异常。其实这时没有有效利用异步，需要自己不停的检查是否成功。并且不能同时处理多个socket的事件。

## 3. selec、epoll、kqueue 轮询

- python3.4 DefaultSelector 会自动取得当前系统最优的轮询事件循环。
- select、epoll、kqueue的接口都差不多

		注册回到函数，取消注册
		from selectors import DefaultSelector, EVENT_WRITE
	
		selector = DefaultSelector()
		
		sock = socket.socket()
		sock.setblocking(False)
		try:
		    sock.connect(('xkcd.com', 80))
		except BlockingIOError:
		    pass
		
		def connected():
		    selector.unregister(sock.fileno())
		    print('connected!')
		
		selector.register(sock.fileno(), EVENT_WRITE, connected)


当在loop中节后到I/O通知时，在IOLoop中处理它。

----------
	def loop():
	    while True:
	        events = selector.select()
	        for event_key, event_mask in events:
	            callback = event_key.data
	            callback()

连接的回调函数存储为event_key.data,一旦非阻塞套接字连接，我们将检索并执行它。selector.select是阻塞的，它会一直阻塞到有事件到来，然后调用callback来处理它。<font color=red>这里的callback还是阻塞的，这里的异步是socket是异步，然后可以同时处理大量的socket句柄。</font>

<font color=blue>**一个异步框架基于两个我们展示的功能：（非阻塞的套接字和事件循环）在单个线程上运行并发操作**</font>

<font color=blue>我们这里已经实现了“并发”，但不是传统上称之为”并行“的东西，也就是说，我们构建了一个可以重叠I/O的小型系统，它有能力在其他操作执行时，启动一个新的操作。它实际上没有利用多个内核来并行执行计算，但是这个系统是为了I/O约束问题而设计的，而不是cpu约束问题</font>

<font color=red>这个事件循环在并发I/O方面效率很高，因为它不会为每个连接投入线程资源，但纠正一个常见的错误认识，即异步比多线程更快。通常情况下，它不是的。在事件循环服务少量非常活跃的连接时比多线程慢，在没有全局解释器锁的运行时中，线程在这样的工作负载上执行的更好。**异步I/O最社和的场景是有许多慢或者睡眠的连接和偶发事件的应用程序 </font>**


## 4 回调

	urls_todo = set(['/'])
	seen_urls = set(['/'])

	class Fetcher:
	    def __init__(self, url):
	        self.response = b''  # Empty array of bytes.
	        self.url = url
	        self.sock = None

		# Method on Fetcher class.
	    def fetch(self):
	        self.sock = socket.socket()
	        self.sock.setblocking(False)
	        try:
	            self.sock.connect(('xkcd.com', 80))
	        except BlockingIOError:
	            pass
	
	        # Register next callback.
	        selector.register(self.sock.fileno(),
	                          EVENT_WRITE,
	                          self.connected)

The fetch method begins connecting a socket. But notice the method returns before the connection is established. It must return control to the event loop to wait for the connection. To understand why, imagine our whole application was structured so:

	# Begin fetching http://xkcd.com/353/
	fetcher = Fetcher('/353/')
	fetcher.fetch()
	
	while True:
	    events = selector.select()
	    for event_key, event_mask in events:
	        callback = event_key.data
	        callback(event_key, event_mask)

All event notifications are processed in the event loop when it calls select. Hence fetch must hand control to the event loop, so that the program knows when the socket has connected. Only then does the loop run the connected callback, which was registered at the end of fetch above.

Here is the implementation of connected:

	# Method on Fetcher class.
    def connected(self, key, mask):
        print('connected!')
        selector.unregister(key.fd)
        request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(self.url)
        self.sock.send(request.encode('ascii'))

        # Register the next callback.
        selector.register(key.fd,
                          EVENT_READ,
                          self.read_response)




<font color=red>一个真正的应用程序会检查发送的返回值，以防一次发送整个消息。但我们的请求很小而且request里会封装这些接口。它发送请求，等待响应。它必须注册另外一个回调函数并将控制权交给事件循环，下个回调函数是read_response处理服务器的回复</font>

    # Method on Fetcher class.
    def read_response(self, key, mask):
        global stopped

        chunk = self.sock.recv(4096)  # 4k chunk size.
        if chunk:
            self.response += chunk
        else:
            selector.unregister(key.fd)  # Done reading.
            links = self.parse_links()

            # Python set-logic:
            for link in links.difference(seen_urls):
                urls_todo.add(link)
                Fetcher(link).fetch()  # <- New Fetcher.

            seen_urls.update(links)
            urls_todo.remove(self.url)
            if not urls_todo:
                stopped = True

The callback is executed each time the selector sees that the socket is "readable", <font color=red>which could mean two things: the socket has data or it is closed.

上面代码socket每次接受4k数据，如果socket数据小于4k则可以将数据读完，如果大于4k，则进入下一个事件循环，会再次将这个socket返回可读，再次读取数据。直到socket关闭也就是可读事件但读取的数据为空，这时才可以人为这次socket数据接收完了，可以解除注册了。</font>

- <font color=green>查一下客户端接受数据时接受到的数据块的大小的处理？</font>

- <font color=green>查一下select方法是水平触发还是边沿触发，如果是边沿触发，那socket仍然可读是不会再返回第二次read信号吧？</font>


	stopped = False
	
	def loop():
	    while not stopped:
	        events = selector.select()
	        for event_key, event_mask in events:
	            callback = event_key.data
	            callback()


这个例子使得async的问题变得简单，你需要某种方式表示一系列的计算和io操作，并调度这些操作并发执行。但是<font color=red>如果没有多线程，这一系列操作不应该在一个函数中：一个函数开始一个I/O操作，它会明确的保存任何在将来需要的状态</font>

Let us explain what we mean by that. Consider how simply we fetched a URL on a thread with a conventional blocking socket:

	# Blocking version.
	def fetch(url):
	    sock = socket.socket()
	    sock.connect(('xkcd.com', 80))
	    request = 'GET {} HTTP/1.0\r\nHost: xkcd.com\r\n\r\n'.format(url)
	    sock.send(request.encode('ascii'))
	    response = b''
	    chunk = sock.recv(4096)
	    while chunk:
	        response += chunk
	        chunk = sock.recv(4096)
	
	    # Page is now downloaded.
	    links = parse_links(response)
	    q.add(links)








参考：[500 lines craw](http://aosabook.org/en/500L/pages/a-web-crawler-with-asyncio-coroutines.html)