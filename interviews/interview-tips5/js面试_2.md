## js_2

[1](https://juejin.im/post/5d9c284b518825095879e7a5)


1. 小程序超过10层，怎么处理
2. http请求，并发次数限制
3. storage 用过哪些，数据过期怎么处理
4. await 报错
5. ttfb
6. 三次握手四次挥手

	
		第一次握手：客户端发送网络包，服务端收到了。
		这样服务端就能得出结论：客户端的发送能力、服务端的接收能力是正常的。
		第二次握手：服务端发包，客户端收到了。
		这样客户端就能得出结论：服务端的接收、发送能力，客户端的接收、发送能力是正常的。不过此时服务器并不能确认客户端的接收能力是否正常。
		第三次握手：客户端发包，服务端收到了。
		这样服务端就能得出结论：客户端的接收、发送能力正常，服务器自己的发送、接收能力也正常。
		
		因此，需要三次握手才能确认双方的接收与发送能力是否正常。
	
	
	
	
		第一次挥手：这表示主机1没有数据要发送给主机2了；
		第二次挥手：主机2收到了主机1发送的FIN报文段，向主机1回一个ACK报文段，Acknowledgment Number为Sequence Number加1；主机1进入FIN_WAIT_2状态；主机2告诉主机1，我“同意”你的关闭请求；
		第三次挥手：主机2向主机1发送FIN报文段，请求关闭连接，同时主机2进入LAST_ACK状态；
		第四次挥手：主机1收到主机2发送的FIN报文段，向主机2发送ACK报文段，然后主机1进入TIME_WAIT状态；
		主机2收到主机1的ACK报文段以后，就关闭连接；此时，主机1等待2MSL后依然没有收到回复，则证明Server端已正常关闭，那好，主机1也可以关闭连接了。
	
	
![img](https://img-blog.csdn.net/20160926193227287?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


 四次挥手释放连接时，等待2MSL的意义?
	
		MSL是Maximum Segment Lifetime的英文缩写，可译为“最长报文段寿命”，它是任何报文在网络上存在的最长时间，超过这个时间报文将被丢弃。
		
		为了保证客户端发送的最后一个ACK报文段能够到达服务器。因为这个ACK有可能丢失，从而导致处在LAST-ACK状态的服务器收不到对FIN-ACK的确认报文。
		
		服务器会超时重传这个FIN-ACK，接着客户端再重传一次确认，重新启动时间等待计时器。最后客户端和服务器都能正常的关闭。
		
		假设客户端不等待2MSL，而是在发送完ACK之后直接释放关闭，一但这个ACK丢失的话，服务器就无法正常的进入关闭连接状态。
	

7. 获取 cookie 思路

	substring() 方法用于提取字符串中介于两个指定下标之间的字符。

		function getCookie(cname){
		    var name = cname + "=";
		    var ca = document.cookie.split(';');
		    for(var i=0; i<ca.length; i++) {
		        var c = ca[i].trim();
		        if (c.indexOf(name)==0) { return c.substring(name.length,c.length); }
		    }
		    return "";
		}	
	
8. 301、302区别：  


	```
	访问 changchen.me 的响应(301):
	HTTP/1.1 301 Moved Permanently
	Server: nginx/1.10.3 (Ubuntu)
	Date: Sun, 13 Aug 2017 05:37:04 GMT
	Content-Type: text/html
	Content-Length: 194
	Connection: keep-alive
	Location: https://changchen.me/
	
	访问 www.changchen.me 的响应(302):
	HTTP/1.1 302 Moved Temporarily
	Server: nginx/1.10.3 (Ubuntu)
	Date: Sun, 13 Aug 2017 05:38:23 GMT
	Content-Type: text/html
	Content-Length: 170
	Connection: keep-alive
	Location: https://changchen.me/
	```
	
	都会返回一个新地址，location

		301 意味着客户端可以对结果进行缓存， 搜索引擎或者浏览器都可以把跳转后的地址缓存下来，下一次不必发送这个请求。  
		302 就是客户端必须请求原链接。	 
		303： 对于POST请求，它表示请求已经被处理，客户端可以接着使用GET方法去请求Location里的URI 
		
		307， http1.1 中的302，临时重定向


	1. 临时：可能下一次访问，返回的 location 就不是 /new 了
	2. 永久: 第二次访问，不需要服务器端查询，在浏览器端把旧地址转为新地址
	而临时重定向，每次还是先去老地址，通过服务端的跳转，然后跳转到新地址
	
	3. 301 的地址会被永久缓存在浏览器端，如果用户不主动清缓存，是不会变的，所以301地址变化需要谨慎处理