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










参考：[500 lines craw](http://aosabook.org/en/500L/pages/a-web-crawler-with-asyncio-coroutines.html)