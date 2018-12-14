每天搞定一点点！加油！

### 1.es6面试题概览  
1. 箭头函数中this的指向问题  
		会继承
2. promise 系列问题  
		event loop问题，和setTimeout 哪个先执行  
		什么是值穿透  
		Promise状态一旦改变，无法在发生变更  
		catch什么时候执行  
3. 数据系列问题 
	... 用法
		

### 2.自己整理的题目+答案

#### 1.es5中的this和es6中的this区别  

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
bind(this)的原理？
 
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


严格模式下的this，undefined    
因为f1定义时所处的函数 中的 this是指的 obj, setTimeout中的箭头函数this继承自f1, 所以不管有多层嵌套,都是 obj

```
var obj = {
     say: function() {
        function f1() {
            console.log(this);
            setTimeout(()=>{
                console.log(this);
            })
        }
        f1();
     }
 }
    obj.say()

```

总结：箭头函数的this继承外面函数的this



### 2. promise

#### 2.1执行顺序问题

	promise 是同步任务  
	then 是异步执行的

#### 2.2值穿透问题  

```
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
  
```
解析  
Promise.resolve 方法的参数如果是一个原始值，或者是一个不具有 then 方法的对象，则 Promise.resolve 方法返回一个新的 Promise 对象，状态为resolved，Promise.resolve 方法的参数，会同时传给回调函数。

then 方法接受的参数是函数，而如果传递的并非是一个函数，它实际上会将其解释为 then(null)，这就会导致前一个 Promise 的结果会穿透下面。




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
[面试题六](http://www.bslxx.com/a/mianshiti/tiku/javascript/2017/1213/1505.html)  

