# 一、什么是Http？
	1.（HyperText Transfer Protocol，超文本传输协议）
	2. 它是TCP/IP协议的一个应用层协议，用于定义WEB浏览器与WEB服务器之间交换数据的过程，默认使用80端口。
	3. 客户端连上web服务器后，若想获得web服务器中的某个web资源，
	   需遵守一定的通讯格式，HTTP协议用于定义客户端与web服务器通迅的格式。

# 二、Http状态码
		1xx：指示信息---表示请求已接收，继续处理

		2xx：成功---表示请求已被成功接收并处理
				200：表明请求成功完成，所有资源成功发送给客户端
				202：服务器已接受请求，但尚未处理。

		3xx：重定向---要完成请求必须进行更进一步的操作
				301：永久重定向
				302：临时重定向
				304：有缓存，且未失效，从本地获取

		4xx：客户端错误---请求有语法错误或请求无法实现
				400：客户端请求有语法错误，不能被服务器解读
				403：服务器拒绝服务（例：http://ehall.szu.edu.cn/new/index.html）
				404：请求失败，请求的资源在服务器上没有找到

		5xx：服务器端错误---服务器未能实现合法的请求
				500：服务器端错误，一般是服务器端代码出错

# 三、为什么需要了解Http缓存？
	  Http缓存是Web性能优化的重要手段

# 四、缓存规则
## 1. 强制缓存
	![compulsion-cache](https://github.com/520203xuxia/HTTP/raw/master/img/compulsion-cache.jpg)

	缓存如果未失效，则直接使用缓存数据。那么怎么判断缓存是否失效呢？
	Http 响应header中，有两个字段标明失效规则：
	* Expires：服务器返回的到期时间，HTTP 1.0的东西，基本忽略它的作用
	* Cache-Control：很重要的规则，默认值为private
		private:		客户端可以缓存
		public:	   		客户端和代理服务器都可缓存（前端的同学，可以认为public和private是一样的）
		max-age=xxx:	缓存的内容将在 xxx 秒后失效
		no-cache:       需要使用对比缓存来验证缓存数据
		no-store:       所有内容都不会缓存，强制、对比缓存都不会触发

	强制缓存生效时：
	![compulsion-cache-example.jpg](https://github.com/520203xuxia/HTTP/raw/master/img/compulsion-cache-example.jpg)

## 2. 对比缓存
	![contrast-cache](https://github.com/520203xuxia/HTTP/raw/master/img/contrast-cache.jpg)

	* 对比缓存，需要进行比较判断是否可以使用缓存。
	* 浏览器第一次请求数据时，服务器会将缓存标识与数据一起返回给客户 端，客户端将二者备份至缓存数据库中。
	* 再次请求数据时，客户端将备份的缓存标识发送给服务器，服务器根据缓存标识进行判断，判断成功后，返回304状态码，通知客户端比较成功，可以使用缓存数据。

	对于对比缓存来说，缓存标识的传递是我们着重需要理解的，它在请求header和响应header间进行传递，一共分为两种标识传递：

	1.	Last-Modified  /  If-Modified-Since
		Last-Modified：
		服务器在响应请求时，告诉浏览器资源的最后修改时间。

		If-Modified-Since：
		再次请求服务器时，通过此字段通知服务器上次请求时，服务器返回的资源最后修改时间。

	2.	Etag/If-None-Match（优先级高于Last-Modified/If-Modified-Since）
		Etag：
		服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识（生成规则由服务器决定）

		If-None-Match：
		再次请求服务器时，通过此字段通知服务器客户段缓存数据的唯一标识。

	对比缓存生效时：



两类缓存规则的不同：
* 强制缓存如果生效，不需要再和服务器发生交互。
* 对比缓存不管是否生效，都需要与服务端发生交互。
* 两类缓存规则可以同时存在，强制缓存优先级高于对比缓存。

	 - **对于强制缓存，服务器通知浏览器一个缓存时间，在缓存时间内，下次请求，直接用缓存，不在时间内，执行比较缓存策略。**

	 - **对于比较缓存，将缓存信息中的Etag和Last-Modified通过请求发送给服务器，由服务器校验，返回304状态码时，浏览器直接使用缓存**


# 五、Http响应头和请求头
## 1. 请求头（Request Headers）
**Host**： 请求报头域主要用于指定被请求资源的Internet主机和端口号，必须值

**Accept**：浏览器可以接受的媒体类型（text/html、*/*）

**Accept-Encoding**：浏览器声明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法(gzip，deflate)

**Accept-Language**：浏览器声明自己接收的语言。语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等（Accept-Language:en-us）

**Connection**：keep-alive，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。如果是Connection：close，代表一个Request完成后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭，当客户端再次放松Request，需要重新建立TCP连接。

**Cache-Control**：通用首部字段。private|public|no-cache|max-age|no-  store，默认为private

**User-Agent**：告诉服务器，客户端使用的操作系统和浏览器的名称和版本。

**Referer**： 当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器我是从哪个页面链接过来的，服务器籍此可以获得一些信息用于处理。比如从我主页上链接到一个朋友那里,他的服务器就能够从HTTPReferer中统计出每天有多少用户点击我主页上的链接访问他的网站。

**Cookie**：用来存储一些用户信息以便让服务器辨别用户身份的。

**Cache-Control**：通用首部字段。 我们网页的缓存控制是由HTTP头中的“Cache-control”来实现的，常见值有private、no-cache、max-age、must-revalidate等，默认为private。

**If-Modified-Since**： 把浏览器端缓存页面的最后修改时间发送到服务器去，服务器会把这个时间与服务器上实际文件的最后修改时间进行对比。如果时间一致，那么返回304，客户端就直接使用本地缓存文件。如果时间不一致，就会返回200和新的文件内容。客户端接到之后，会丢弃旧文件，把新文件缓存起来，并显示在浏览器中。

**If-None-Match**： If-None-Match和ETag一起工作，工作原理是在HTTP Response中添加ETag信息。 当用户再次请求该资源时，将在HTTP Request 中加入If-None-Match信息(ETag的值)。如果服务器验证资源的ETag没有改变（该资源没有更新），将返回一个304状态告诉客户端使用本地缓存文件。否则将返回200状态和新的资源和Etag.  使用这样的机制将提高网站的性能。

**Proxy-Connection**：


## 2. 响应头（Response Headers）
**Server**：响应客户端的服务器。

**Date**：响应的时间。

**Connection**：keep-alive，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。如果是Connection：close，代表一个Request完成后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭，当客户端再次放松Request，需要重新建立TCP连接。

**Proxy-Connection**：

**Pragma**：HTTP 1.0的遗留物，值为”no-chche“时禁用缓存

**ETag**：服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识

**Expires**：为服务端返回的到期时间，即下一次请求时，请求时间小于服务器返回的到期时间，直接使用缓存数据，但这是HTTP 1.0的东西，现在浏览器默认使用HTTP 1.1，所以他的作用基本忽略。

**Cache-Control**：通用首部字段。private|public|no-cache|max-age|no-  store，默认为private

	private：客户端可以缓存

	public：客户端和代理服务器都可缓存（对前端开发者来说，认为public和private是一样的）

	max-age=xxx：缓存的内容将在xxx秒后失效

	no-cache：内容会被缓存，只是每次使用都需要向服务器验证其有效性

	no-store：所有内容都不会缓存，强制缓存，对比缓存都不会触发（但对于前端开发来说，缓存越多越好，所以这个值基本不用）

**Last-Modified**：服务器在响应请求时，告诉浏览器资源的最后修改时间。

**Content-Type**：响应正文的类型。（图片还是二进制字符串）

**Content-Length**：响应正文长度。

**Content-Charset**：响应正文使用的编码。

**Content-Encoding**：响应正文使用的数据压缩格式

**Content-Language**：响应正文使用的语言

**Access-Control-Allow-Origin**：

**Access-Control-Allow-Methods**：


	请求头和相应头中都有Cache-Control字段，有什么区别？

	响应头中的这个属性是服务器端告诉客户端应该怎么做，做不做还是决定于客户端。

	如何理解请求头中Cache-Control：max-age=0，而响应头中Cache-Control：max-age=3600?

	客户端强制要求服务端返回最新文件，响应头中是服务器端告诉客户端，
	该文件在3600s内不用再向服务器端请求了。但如果请求头下次请求时
	该字段值为max-age=0，则就必须由服务器返回最新文件。

	响应头中Cache-Control：max-age=0。代表服务器要求浏览器你在
	使用本地缓存的时候，必须先和服务器进行一遍通信，将etag、
	 If-Not-Modified等字段传递给服务器以便验证当前浏览器端使用
	 的文件是否是最新的（如果浏览器用的是最新的文件，http状态码
	 返回304，服务器告诉浏览器使用本地缓存即可；否则返回200，
	 浏览器得自己把文件重新下载一遍）。

