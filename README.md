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

![301-jpg](https://520203xuxia.github.io/HTTP/raw/master/img/status-code-301.jpg)
