

## 问题
1. 什么是深拷贝？什么是浅拷贝？
2. 哪些方法实现浅拷贝，哪些方法实现深拷贝？
3. 深拷贝递归停止的条件是什么？
4. $.extend() 、 Object.assign()  以及
loadash 插件的_.clone() 、_.cloneDeep() 函数是如何实现拷贝的？
5. 尾递归如何实现深复制？


## 思考
### 1.基本类型和引用类型区别
基本类型保存在栈中，保存的是值  
引用类型保存在堆中，栈中存放的是数据在堆中的地址，使用的时候，我们先从栈中取出地址，然后根据地址去堆中找相应的数据。    

### 2.浅拷贝和深拷贝区别
名字联想，浅拷贝就是，浅显简单拷贝，但是会有问题  
什么问题？拷贝基本类型就是拷贝值，改A，B不会跟着变， 但涉及到拷贝引用类型的数据时，拷贝的是数据的地址，修改原数据时候，拷贝的数据也会跟着修改  

深拷贝，递归拷贝，所有拷贝都是拷贝值，没有拷贝地址，你变，我不变

### 3.浅拷贝
#### 3.1 浅拷贝方法：
1. Array.prototype.slice();  
2. Array.prototype.concat();  
3. Object.assign;  
4. 直接拷贝
	
```js
	//slice concat 
	var arr = [
	  {number:1},
	  {number:2},
	  {number:3}
	];
	var copyArr = arr.slice();
	//var copyArr = arr.concat();
	copyArr[0].number = 10;
	console.log(copyArr);  // [{number: 10}, { number: 2 },{ number: 3 }]
	console.log(arr); // [{number: 10}, { number: 2 }, { number: 3 }]
			
		
	//直接拷贝
	var obj = {
	add: {
		prov: 'anhui',
		city: 'hefei'
	},
	job: "coder"
	};
	
	function copy (arg) {
		let newobj = {}
		for(let item in obj) {
		  newobj[item] = obj[item];
		}
		return newobj;
	}
	
	var copyobj = copy(obj);
	copyobj.add.prov = 'shanghai';
	console.info(copyobj);  
	console.info(obj);  //prov变成shanghai了
	
	
	
	// Object.assign
	let obj2 = Object.assign({}, obj);
	obj2.add.prov = 'shanghai';
	console.log(obj2); 
	console.log(obj); //prov变成shanghai
	  
```
	
#### 3.2 浅拷贝原因：
为什么是浅拷贝，我们看slice源码：

concat源码：
	
Object.assign源码：
	
	
可以他们相当于都是，对每个子元素，使用的直接赋值

```
	for(let item in obj) {
		newobj[item] = obj[item];
	}
```
所以是浅拷贝
	

### 4.深拷贝
#### 4.1 深拷贝方法：
	1. JSON.parse（有一定的局限性）
	2. 递归拷贝


#### 4.2 具体实现
1. JSON.parse 实现深拷贝，如何实现的，不足之处？  
	会忽略 undefined  
	会忽略symbol  
	不能序列化函数  
	不能解决循环引用的对象	  	
	 
	```js
	let obj = {
	    add: {
	    	prov: 'anhui',
	    	city: 'hefei'
	    },
	    job: "coder",
	    girlfriend: undefined,
	    birth: new Date('1990-01-01'),
	    eat: function() {
	    	console.log("妹子喜欢吃牛排");
	    }
	  };
	  
	let obj3 = JSON.parse(JSON.stringify(obj));
	obj3.add.prov = 'beijing';
	console.log(obj);
	console.log(obj3);  //没有eat、girlfriend
	```

2. 递归拷贝实现深拷贝

	如果复制的值中不包含 RegExp 或者 Date 特殊Object类型，用写法1就够了 
	 
	如果还需对特殊类型的属性值单独处理，如Date、Function类型的数据，循环引用问题，见写法2
	
	```js
	let obj = {
	    add: {
	    	prov: 'anhui',
	    	city: 'hefei'
	    },
	    job: "coder",
	    // girlfriend: undefined,
	    birth: new Date('1990-01-01'),
	    regExp: /test/ig,
	    eat: function() {
	    	console.log("妹子喜欢吃牛排");
	    }
	  };
	  
	 //写法1
	 function deepClone(obj){
	  var newobj = obj.constructor === Array ? [] : {}; 
	  for(let i in obj) {
	  	if(obj.hasOwnProperty(i)) {
	  		if(newobj[i] && typeof(obj[i]) === 'object') {
	  			newobj[i] = deepClone(obj[i])
	  		}
	  		else {
	  			newobj[i] = obj[i];
	  		}
	  	}
	  }
	  return obj;
	 }
	 	 
	 		
		
	let obj4 = deepCopy(obj);
	obj4.add.prov = 'beijing';
	console.log('obj', obj);
	console.log('obj4', obj4); 
	```
3. jquery 中怎么实现深拷贝的

	我们可以使用 $.extend(true, {}, obj)实现深拷贝  
	$.extend 的第一个参数 true 表示递归调用  
	$.extend() 源码，参考 [extend源码](https://github.com/jquery/jquery/blob/1472290917f17af05e98007136096784f9051fab/src/core.js#L121)  
	
	```js
	var x = {
	    a: 1,
	    b: { f: { g: 1 } },
	    c: [ 1, 2, 3 ]
	};
	
	var y = $.extend({}, x),          //shallow copy
	    z = $.extend(true, {}, x);    //deep copy
	
	y.b.f === x.b.f       // true
	z.b.f === x.b.f       // false
	
	```
4. loadash 实现深复制  
在lodash中关于复制的方法有两个，分别是_.clone()和_.cloneDeep()。其中_.clone(obj, true)等价于_.cloneDeep(obj)。使用上，lodash和前两者并没有太大的区别，但看了源码会发现，Underscore 的实现只有30行左右，而 jQuery 也不过60多行。可 lodash 中与深复制相关的代码却有上百行，这是什么道理呢？
主要是为了处理深拷贝中源对象内部循环引用，例如：

	
	```
	var $ = require("jquery"),
    _ = require("lodash");
    
   var obj = {
	    a: 1,
	    b: { f: { g: 1 } },
	    c: [ 1, 2, 3 ]
	};
	obj2 = _.cloneDeep(obj); 
	
	
	//直接报了栈溢出。Uncaught RangeError: Maximum call stack size exceeded
	var a = {"name":"aaa"};
	var b = {"name":"bbb"};
	a.child = b;
	b.parent = a;
	$.extend(true,{},a); 	
	```
怎么解决的，有兴趣的自己看吧



## 参考资料
[参考一](https://juejin.im/post/5beb93de6fb9a049c30ac9ee)    
[参考二](https://juejin.im/post/5b5dcf8351882519790c9a2e)  
[参考三](https://juejin.im/post/5b304fbef265da59a76c94a6)    
[参考四](https://github.com/junhey/junhey.github.io/issues/2)  
[参考五](http://jerryzou.com/posts/dive-into-deep-clone-in-javascript/)  
[参考六](https://github.com/wengjq/Blog/issues/3)  
[参考七](https://juejin.im/post/5bed21ec51882517193551e3#heading-6)  
[参考八](https://juejin.im/post/5b5d3f54e51d453467551604#heading-11)  
