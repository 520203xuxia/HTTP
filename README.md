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

	对于强制缓存，服务器通知浏览器一个缓存时间，在缓存时间内，下次请求，直接用缓存，不在时间内，执行比较缓存策略。

	对于比较缓存，将缓存信息中的Etag和Last-Modified通过请求发送给服务器，由服务器校验，返回304状态码时，浏览器直接使用缓存


