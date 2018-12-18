每天搞定一点点！加油！  
才疏学浅，如有错误，希望指出

### 1.es6面试题概览  
1. 箭头函数中this的指向问题  
		会继承
2. promise 系列问题  
	* event loop问题，和setTimeout 哪个先执行  
	* 什么是值穿透  
	* Promise状态一旦改变，无法在发生变更  
	* catch什么时候执行  
3. 数据系列问题 
	... 用法
		

### 2.自己整理的题目+答案

#### 1.es5中的this和es6中的this区别  
es5 中 this 知识点：  
	1. 全局变量默认挂载在window对象下  
	2. 在普通函数中,this指向它的直接调用者;如果找不到直接调用者,则是window  


先看看以下代码会输出什么

```
var obj = {
    say: function() {
        setTimeout(function() {
            console.log(this);
        },0)
    }
}
obj.say();

//window
匿名函数,定时器中的函数,由于没有默认的宿主对象,所以默认this指向window
```
如果想让this = obj 怎么做？  
1：先用that把this保存起来  
2：或者使用 bind(this)  
bind(this)的原理？bind绑定宿主对象后依然返回这个函数, bind还可以返回？

 
```
var obj = {
    func: function() {
    },
    say: function() {
        var that = this;
        setTimeout(function() {
            that.func();
            console.log(that);
        });
    }
}


var obj = {
    func: function() {
    },
    say: function() {
        setTimeout(function(){
            console.log(this);
        }.bind(this));
    }
}

```



	window.val = 1;
	 var obj = {
	   val: 2,
	   dbl: function () {
	     this.val *= 2;
	     val *= 2;
	     console.log(val);
	     console.log(this.val);
	   }
	 };
	 // 说出下面的输出结果
	 obj.dbl();
	 var func = obj.dbl;
	 func();
	 
	 结果是:  2   4    8   8

	<1> 12行代码调用
	
	val变量在没有指定对象前缀,默认从函数中找,找不到则从window中找全局变量
	
	即 val *=2 就是 window.val *= 2
	
	this.val默认指的是 obj.val ;因为 dbl()第一次被obj直接调用
	
	<2>14行代码调用
	
	func() 没有任何前缀,类似于全局函数,即  window.func调用,所以
	
	第二次调用的时候, this指的是window, val指的是window.val
	
	第二次的结果受第一次的影响




<b>严格模式下的this，undefined  </b>  
注意以下几种写法，this为什么不同

```
var obj = {
say: function () {
  var f1 = () => {
    console.log(this); // obj
    setTimeout(() => {
      console.log(this); // obj
    })
  }
  f1();
  }
}
obj.say();


var obj = {
say: function () {
  var f1 = function () {
    console.log(this); // window, f1调用时,没有宿主对象,默认是window
    setTimeout(() => {
      console.log(this); // window
    })
  };
  f1();
  }
}
obj.say();


var obj = {
say: function () {
  'use strict';
  var f1 = function () {
  console.log(this); // undefined
  setTimeout(() => {
    console.log(this); // undefined
  })
  };
  f1();
 }
}
obj.say()


```

总结：箭头函数的this继承外面函数的this



### 2. promise

#### 2.1 概念问题
	promise 为了解决啥？
	为了解决之前出现的回调地域问题，在异步请求结果中再次发起异步请求，造成层层嵌套。
	
	promise，可以在then方法中继续写Promise对象并返回，然后继续调用then来进行回调操作!!!!
	
	// 第一次 
	$.ajax({
	     url:'/xxx',
	     success:()=>{
	         // 第二次
	         $.ajax({
	             url:'/xxx',
	             success:()=>{
	               // 第三次
	               $.ajax({
	                  url:'/xxx',
	                  success:()=>{
	                   // 可能还会有
	                  },
	                  error: ()=>{}
	                })
	             },
	             error: ()=>{}
	        })
	     },
	     error: ()=>{}
	})
	
	$.ajax({
    	url:'/xxx',
	}).then( ()=>{
	   // 成功的回调
	}, ()=>{
	  // 失败的回调 
	})
	
	
	resolve 和 fulfilled 作用
	Promise.resolve 返回一个fulfilled状态的promise对象，Promise.reject 返回一个rejected状态的promise对象。


#### 2.2执行顺序问题

	promise 是同步任务, 立即执行  
	then 是异步执行的
	宏任务、微任务、执行栈、执行队列
	
	
	setTimeout(()=>{
	  console.log('setTimeout')
	})
	let p1 = new Promise((resolve)=>{
	  console.log('Promise1')
	  resolve('Promise2')
	})
	p1.then((res)=>{
	  console.log(res)
	})
	console.log(1)

#### 2.3值穿透问题  

```
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
  
```
解析  
Promise.resolve 方法的参数如果是一个原始值，或者是一个不具有 then 方法的对象，则 Promise.resolve 方法返回一个新的 Promise 对象，状态为resolved，Promise.resolve 方法的参数，会同时传给回调函数。

then 方法接受的参数是函数，而如果传递的并非是一个函数，它实际上会将其解释为 then(null)，这就会导致前一个 Promise 的结果会穿透下面。  

解析参考：[深入 Promise(一)——Promise 实现详解](http://blog.51cto.com/12405360/1897688)


#### 2.4其他问题
	1.then 返回的对象和 在 then 中返回的 new promise 对象有啥不同啊
	
	//promise 学习
	{
	  let fn = new Promise(function(resolve, reject) {
	    let num = 3;
	    if (num > 5) {
	      resolve(num)
	    } else {
	      reject(num)
	    }
	  })
	  // 第一次回调
	  fn.then((res) => {
	    console.log(`第一次回调成功res==>${res}`);
	    return new Promise((resolve, reject) => {
	      if (2 * res > 15) {
	        resolve(2 * res)
	      } else {
	        reject(2 * res)
	      }
	    })
	  }, (err) => {
	    console.log(`第一次回调失败err==>${err}`);
	  }).then((res) => { // 第二次回调
	    console.log(`第二次回调失败${res}`);
	  }, (err) => {
	    console.log(`第二次回调失败err==>${err}`);
	  });
	
	}
	
	2. 隐形返回对象和返回数据，有啥区别啊？
	Promise.resolve(1)
    .then((res) => {
        console.log(res);
        return 2;
    })
    .catch((err) => {
        return 3;
    })
    .then((res) => {
        console.log(res);
    });
    //输出结果：1  2
    
#### 2.5 动手系列
	实现一个简单的promise
	实现红绿灯
	实现 mergePromise 函数，把传进去的数组按顺序先后执行，并且把返回的数据先后放到数组 data 中。



### 3. 数组系列问题
var a = "abc";
	console.log(...a);

	function b(...arg) {
		console.log(arg);
	}






### 网上搜集的es6题
[面试题一](https://blog.csdn.net/nimeghbia/article/details/80733418)  
[面试题二](https://www.cnblogs.com/lunlunshiwo/archive/2018/04/16/8852984.html)  
[面试题三](https://segmentfault.com/a/1190000016848192)  
[面试题四](https://www.cnblogs.com/fengxiongZz/p/8191503.html)  
[面试题五](https://segmentfault.com/a/1190000011344301)  

