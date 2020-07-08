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


## 9. 缓存区别，

### todo interview-tips3- 3 -3章

vuex：存在虚拟内存中，页面刷新就没有了

用户登录，用户信息，不要存在localStorage，使用 vuex存储，

1. 存在本地，明文存储，不安全。

2. 用户信息更新频繁
localStorage：

持久化存储，页面刷新还是回存在
防止页面经常刷新，页面变化，数据还存在
比如首页信息，放在localStorage 中，不会每次刷新就变化
但是有个缺点，是永久存储，数据不会变
每次存储数据，设置存储时间，下次拿的时候，拿当前时间和存储时间做对比，如果超过10min, 从服务器拿，做持久化存储的过期限制



## 10. interview4 - 3

1. beforeCreate，实例初始化在这个生命周期遍历 data 对象下所有属性将其转化为 getter/setter,  无data, 

2. created，实例已经被创建完毕 属性已经绑定 属性是可以操作的, 有data。在控制台打印data ，可以访问到属性了

3. beforeMount， 模板编译， el还未对数据进行渲染 还是不能操作dom，这个时候不能访问 ```$ref, $el``` 原生dom


4. mounted, 页面渲染完，数据挂载到 dom 上  ，控制台打印出dom。	
	对象页面渲染完毕，可以ajax请求，或者绑定事件	

5. beforeUpdate， data被修改，dom没有修改	
6. updated， 虚拟dom重新渲染并应用更新	

7. beforeDestroy，销毁绑定事件监听	
	
 	beforeDestroy钩子函数在实例销毁之前调用。在这一步，实例仍然完全可用。
 	
 	比如消除 setTimout 定时任务，绑定事件

8. destroyed钩子函数在Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

 
父 beforeCreate > 父created > 父 beforMount , 子组件beforeCreate ， 子组件created > 子组件beforMount， 子组件mounted，  父组件 mounted




## 11.  何时使用 beforeDestory

		1. 解绑自定义事件， event.$off, 
		
		2. 清楚定时器
		
		3. 解绑自定义的dom 事件，addEventLisener, 如 window scroll


## 12  module chunk bundle 区别

module： webpack 中一切皆模块， 只有可以被引用的文件，都是模块

chunk： 多模块合并成的， 如 entry import() splitChunk

> chunk三大来源
	
	entry
	splitChunks
	import()
	
	
	
是一系列模块文件生成的文件
	
	比如 entry index.js 文件，可能不止index.js 这一个文件，把这个页面引用的 其他文件，生成 chunk
	
	比如异步引入文件，import().then 生成一个 chunk
	
	比如，把所有的第三方库，生成一个 chunk
	
	把 公共模块，生成一个chunk
	
bundle： 最终的输出文件，在 dist 文件下生成的文件。


## 13.webpack 优化

dev 环境，构建速度：
	
	更小
		因为uglifyjs不支持es6语法，所以用terser-webpack-plugin替代uglifyjs-webpack-plugin
		TerserJSPlugin，js压缩
		OptimizeCSSAssetsPlugin， css压缩
		ignorePlugin，不引入无用模块，比如引入 moment日期内库，默认引入所有语音js代码，只引入中文、英文即可
		noparse，引入不编译, 不进行模块化分析
	
	更快
		happyPack  多进程打包，注意不是多线程。 
		paralleUglify  多进程压缩js
	
	缓存
		优化 babel-loader，开启缓存，cacheDirectory，明确范围，include，exclude，不用每次重新编译
		DllPlugin  比较大的第三方库，不用每次打包都打包一次
	
	性能
		自动刷新
		热更新  浏览器不刷新，代码已生效


生产环境：

	代码分割
		第三方代码，公共代码抽出， splitchunks, cacheGroups
	
	tree shaking
		使用production
		
		自动代码压缩
	
	异步加载
		懒加载，组件的异步加载
		
	优化
		bundle 加 hash， 内容hash编码 [name].[contentHash:8].js
		小图base64 编码  		
		Scope Hosting
	更小
		ignorePlugin, 打包代码更少
		
	更快
		使用 CDN 加速, 把打包文件，放到 cdn 服务器上去，让 publicPath 可访问
		作用：让所有静态文件 url 的前缀
		
		output: {
		    filename: '[name].[contentHash:8].js', // name 即多入口时 entry 的 key
		    path: distPath,
		    // publicPath: 'http://cdn.abc.com'  // 修改所有静态文件 url 的前缀（如 cdn 域名），这里暂时用不到
		},

## 14. es6 commonjs

ES6 module 静态引入，编译时引用
	
	无条件引入
	打包的时候，能直接识别到
	不能在条件中引用，不能通过代码判断，是否能用
	
commonjs 动态引入，执行时引入
	
	打包的时候不知道是啥，只有执行的时候才能引用
	
只有 es6 module 才能静态分析， 实现 tree shaking

webpack打包的时候，代码还没有执行，只是静态分析，代码还没有正式被使用

```
let apiList = require('../config/api.js')
if(isDev) { 
  //执行的时候才知道，里面的代码是否会执行
  // 执行的时候才知道 isDev 是否执行
  apiList = require('../config/api_dev.js')
  
  //编译时候回报错，只能静态引入
  import apiList from '../config/api.js'
}


import apiList from '../config/api.js'

import不能在逻辑判断里面。在编译时就知道，是否能引入。

```
## 15. 为何 proxy 不能被 polyfill

		class 可以通过 function模拟  
		promise可以使用callback 模拟  
		proxy 无法使用 Object.defineProperty 无法模拟
## 16. 懒加载

	rect = item.getBoundingClientRect() //元素相对视图窗口的位置

	if(rect.bottom > 0 && rect.top < document.clientHeight) {
	或者
	rect.top > 0
	
	4. preload.js 库

	什么时候执行预加载？
	
	 window.onload = function() { 
	 	addLoadEvent(preloader);
	 }
## 17. 如何写一个loader

需求：我们需要替换js文件中的字段

console.log('hello lee');

	如把 lee 替换成 world


loaders 文件夹中，replaceLoader.js 中写

	
	module.exports = function(source) {
		return source.replace('lee', 'world');
	}
2. 网站出中文版，外国版，实现网站国际化
	我们根据某个全局变量，判断是中文还是英文，打包生成不同版本
	
		
		if(node 全局变量 === “中文”) {
			source.replace('{{title}}', '中文标题')
		}else {
			source.replace('{{title}}', '英文标题')
		}
