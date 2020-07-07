## js_1

1. 如何用dom获取一个元素

		document.getElementById
		document. getElementsByClassName
		document.getElementsByTagName
		
		
		document.querySelector("#demo");  //单个元素
		document.querySelectorAll(".example");
		
		
		querySelectorAll 返回的是一个 Static Node List，而 getElementsBy 系列的返回的是一个 Live Node List。


2. event loop

	[这一次，彻底弄懂 JavaScript 执行机制](https://juejin.im/post/59e85eebf265da430d571f89)
	
	哪些同步，哪些异步方法
	
	
		
		process.nextTick(() => {
		  console.log('nextTick')
		})
		Promise.resolve()
		  .then(() => {
		    console.log('then')
		  })
		setImmediate(() => {
		  console.log('setImmediate')
		})
		console.log('end') 
	   
	运行结果
	
		end
		nextTick
		then
		setImmediate
	
	原因
	process.nextTick 和 promise.then 都属于 microtask，而 setImmediate 属于 macrotask，在事件循环的 check 阶段执行。事件循环的每个阶段（macrotask）之间都会执行 microtask，事件循环的开始会先执行一次 microtask。   
	
	
	主线程执行栈空，去 异步队列 看。
	
		promise 是同步任务, 立即执行  
		then 是异步执行的
		宏任务、微任务、执行栈、执行队列
		
		macro-task(宏任务)：包括整体代码script，setTimeout，setInterval
		micro-task(微任务)：Promise.then，process.nextTick	
	判断结果	
		
		
		setTimeout(function() {
		    console.log('setTimeout');
		})
		
		new Promise(function(resolve) {
		    console.log('promise');
		}).then(function() {
		    console.log('then');
		})

		console.log('console');
		
	这段代码作为宏任务，进入主线程。	
	先遇到setTimeout，那么将其回调函数注册后分发到宏任务Event Queue。(注册过程与上同，下文不再描述)		
	接下来遇到了Promise，new Promise立即执行，then函数分发到微任务Event Queue。		
	遇到console.log()，立即执行。好啦，整体代码script作为第一个宏任务执行结束，看看有哪些微任务？我们发现了then在微任务Event Queue里面，执行。	
	ok，第一轮事件循环结束了，我们开始第二轮循环，当然要从宏任务Event Queue开始。我们发现了宏任务Event Queue中setTimeout对应的回调函数，立即执行。
	结束。		
		

3. AST
4. http、https 有什么区别？
5. Vue 组件间通信方式
	
		
6. 预加载是什么时候到什么时候？
7. proxy解决什么
8. 小程序 h5区别
9. common.js 和 import 有什么区别？
	
		1. 动态引入、静态引入
		2. 同步引入，异步引入
		3. 浅拷贝，更新
		4. 转成 import
10. 并发6个请求，限制如何做到？如果限制一分钟限制10个请求？
	
	[并发](https://segmentfault.com/a/1190000016848192#item-8)
	
		题目的意思是需要我们这么做，先并发请求 3 张图片，当一张图片加载完成后，又会继续发起一张图片的请求，让并发数保持在 3 个，直到需要加载的图片都全部发起请求。
	
		用 Promise 来实现就是，先并发请求3个图片资源，这样可以得到 3 个 Promise，组成一个数组，就叫promises 吧
		然后不断的调用 Promise.race 来返回最快改变状态的 Promise，然后从数组（promises ）中删掉这个 Promise 对象，再加入一个新的 Promise，直到全部的 url 被取完，最后再使用 Promise.all 来处理一遍数组（promises ）中没有改变状态的 Promise。


	
	
	
	
11. 全局对象

		浏览器里面 window 就是 global 
		nodejs 里没有 window，但是有个叫 global 的

12. 类型转换
13. this到底指向谁

	
		匿名函数,定时器中的函数,由于没有默认的宿主对象,所以默认this指向window

14.  promise 值穿透问题  

	

	```
	Promise.resolve(1)
	  .then(2)
	  .then(Promise.resolve(3))
	  .then(console.log)
	  
	```
	解析  
	Promise.resolve 方法的参数如果是一个原始值，或者是一个不具有 then 方法的对象，则 Promise.resolve 方法返回一个新的 Promise 对象，状态为resolved，Promise.resolve 方法的参数，会同时传给回调函数。
	
	then 方法接受的参数是函数，而如果传递的并非是一个函数，它实际上会将其解释为 then(null)，这就会导致前一个 Promise 的结果会穿透下面。  
	

		.then 或者 .catch 的参数期望是函数，传入非函数则会发生值穿透。
	
	解析参考：[深入 Promise(一)——Promise 实现详解](http://blog.51cto.com/12405360/1897688)







15. sleep 函数
	
	```
	const timeout = ms => new Promise((resolve, reject) => {
	    setTimeout(() => {
	        resolve();
	    }, ms);
	});
	```

16. 预加载是什么时候到什么时候？
17. 给一个dom添加class， jquery怎么添加的，classList 怎么做的，addClass

		$("p:first").addClass("intro");
		
		
		let div = document.querySelector('div');
		div.classList.add("newClass");
		div.classList.remove("newClass");
18. dom到页面顶部的位置
19. 读取和修改cookie

20. 第一个div 元素
	
	:nth-of-type(n) 选择器匹配属于父元素的特定类型的第 N 个子元素的每个元素.
		
		div:nth-of-type(1)
		{
		background:#ff0000;
		}
