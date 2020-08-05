### 1章 基础知识

1.1 js是动态语言

静态类型语言在编译时便已确定变量的类型，而动态类型语言的变量类型要到程序运行的时候，待变量被赋予某个值之后，才会具有某种类型。

静态优点：编译的时候就能发现可能的错误



1.2 多态

多态的实际含义是：同一操作作用于不同的对象上面，可以产生不同的解释和不同的执行结果。

```
var googleMap = { 
 show: function(){ 
 	console.log( '开始渲染谷歌地图' ); 
 } 
}; 

var baiduMap = { 
 show: function(){ 
 	console.log( '开始渲染百度地图' ); 
 } 
}; 

var renderMap = function( type ){ 
 if ( type === 'google' ){ 
 googleMap.show(); 
 }else if ( type === 'baidu' ){ 
 baiduMap.show(); 
 } 
}; 

renderMap( 'google' ); // 输出：开始渲染谷歌地图
renderMap( 'baidu' ); // 输出：开始渲染百度地图
```

可以看到，虽然 renderMap 函数目前保持了一定的弹性，但这种弹性是很脆弱的，一旦需要

替换成搜搜地图，那无疑必须得改动 renderMap 函数，继续往里面堆砌条件分支语句。

我们还是先把程序中相同的部分抽象出来，那就是显示某个地图：

```
var renderMap = function( map ){ 
 if ( map.show instanceof Function ){ 
	 map.show(); 
 } 
}; 
renderMap( googleMap ); // 输出：开始渲染谷歌地图
renderMap( baiduMap ); // 输出：开始渲染百度地图
```

对象的多态性提示我们，“做什么”和“怎么去做”是可以分开的，即使以后增加了搜搜地图，renderMap 函数仍

然不需要做任何改变



1.4 原型模式和基于原型继承的 JavaScript 对象系统

实现继承：子类对象的 `__proto__` 指向父类的实例，或者，子类构造函数的 prototype 指向父类函数

```
var clonePlane = Object.create( plane ); 
```

或者：

```js
function clone(parent) {
	let fn = function() {};
	fn.prototype = parent;
	return new fn()
}
```



获取原型

```
var obj1 = new Object(); 
Object.getPrototypeOf( obj1 ) === Object.prototype
```

## 2章 this、call、apply

2.1.1 this的指向

除去不常用的 with 和 eval 的情况，具体到实际应用中，this 的指向大致可以分为以下 4 种。

1. 作为对象的方法调用。
2. 作为普通函数调用。
3. 构造器调用。
4. Function.prototype.call 或 Function.prototype.apply 调用。



2. 作为普通函数调用

当函数不作为对象的属性被调用时，也就是我们常说的普通函数方式，此时的 this 总是指向全局对象。在浏览器的 JavaScript 里，这个全局对象是 window 对象。

匿名函数、setTimeout 等，this 指向 window。

```
window.name = 'globalName'; 
var myObject = { 
 name: 'sven', 
 getName: function(){ 
 		return this.name; 
 } 
}; 
var getName = myObject.getName; 
console.log( getName() ); // globalName
```

2.2.1 call和apply的区别

Function.prototype.call 和 Function.prototype.apply 都是非常常用的方法。它们的作用一模一样，区别仅在于传入参数形式的不同。

```javascript
Function.prototype.bind = function(){ 
	var self = this, // 保存原函数
	context = [].shift.call(arguments),
	args = [].slice.call( arguments ); 
	return function(){ // 返回一个新的函数
		// 需要绑定的 this 上下文 // 剩余的参数转成数组
		return self.apply( context, [].concat.call( args, [].slice.call( arguments ) ) ); 
		// 执行新的函数的时候，会把之前传入的context 当作新函数体内的 this
		// 并且组合两次分别传入的参数，作为新函数的参数
	} 
};


function foo(name) {
    this.name = name;
}
var obj = {}
    
//上下文 功能  done
var bar = foo.bind(obj)
bar('jack')
    
// 参数 功能   done
var tar = foo.bind(obj, 'rose');
tar()
```
我们通过 Function.prototype.bind 来“包装”func 函数，并且传入一个对象 context 当作参 数，这个 context 对象就是我们想修正的 this 对象。

如何判断是不是 node 环境？


## 3章 闭包和高阶函数

**3.1.1 变量的作用域**

变量的作用域，就是指变量的有效范围。我们最常谈到的是在函数中声明的变量作用域。



**3.1.2 变量的生存周期**

对于全局变量来说	

​	全局变量的生存周期当然是永久的，除非我们主动销毁这个全局变量。



而对于在函数内用 var 关键字声明的局部变量来说，当退出函数时，这些局部变量即失去了它们的价值，它们都会随着函数调用的结束而被销毁



闭包中变量的生命周期：

这是因为当执行 ```var f = func();```时，f 返回了一个匿名函数的引用，它可以访问到 func() 被调用时产生的环境，而局部变量 a 一直处在这个环境里。既然局部变量所在的环境还能被外界 访问，这个局部变量就有了不被销毁的理由。在这里产生了一个闭包结构，局部变量的生命看起来被延续了。



**3.1.3 闭包的更多作用**

1. 封装变量

闭包可以帮助把一些不需要暴露在全局的变量封装成“私有变量”。



2. 延续局部变量的寿命

img 对象经常用于进行数据上报，如下所示：

```
var report = function( src ){ 
 var img = new Image(); 
 img.src = src; 
}; 

report( 'http://xxx.com/getUserInfo' ); 
```

但是通过查询后台的记录我们得知，因为一些低版本浏览器的实现存在 bug，在这些浏览器 下使用 report 函数进行数据上报会丢失 30%左右的数据，也就是说，report 函数并不是每一次 都成功发起了 HTTP 请求。丢失数据的原因是 img 是 report 函数中的局部变量，当 report 函数的 调用结束后，img 局部变量随即被销毁，而此时或许还没来得及发出 HTTP 请求，所以此次请求 就会丢失掉。


现在我们把 img 变量用闭包封闭起来，便能解决请求丢失的问题:

```javascript
var report = (
	function(){ 
	var imgs = [];
	return function( src ){
		var img = new Image(); 
		imgs.push( img ); 
		img.src = src;
	} 
})();
```

**3.1.6 闭包与内存管理**

局部变量本来应该在函数退出的时候被解除引用，但如果局部变量被封闭在闭包形成的环境 中，那么这个局部变量就能一直生存下去。从这个意义上看，闭包的确会使一些数据无法被及时 销毁。

使用闭包的一部分原因是我们选择主动把一些变量封闭在闭包中，因为可能在以后还需要使用这些变量，把这些变量放在闭包中和放在全局作用域，对内存方面的影响是一致的，这里并 不能说成是内存泄露。如果在将来需要回收这些变量，我们可以手动把这些变量设为 null。

跟闭包和内存泄露有关系的地方是，使用闭包的同时比较容易形成循环引用，如果闭包的作用域链中保存着一些 DOM 节点，这时候就有可能造成内存泄露。但这本身并非闭包的问题，也并非 JavaScript 的问题。

在 IE 浏览器中，由于 BOM 和 DOM 中的对象是使用 C++以 COM 对象 的方式实现的，而 COM 对象的垃圾收集机制采用的是引用计数策略。在基于引用计数策略的垃 圾回收机制中，如果两个对象之间形成了循环引用，那么这两个对象都无法被回收，但循环引用造成的内存泄露在本质上也不是闭包造成的。

同样，如果要解决循环引用带来的内存泄露问题，我们只需要把循环引用中的变量设为 null 即可。将变量设置为 null 意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运 行时，就会删除这些值并回收它们占用的内存。


**函数柯里化**

currying 又称部分求值。一个 currying 的函数首先会接受一些参数，接受了这些参数之后， 该函数并不会立即求值，而是继续返回另外一个函数，刚才传入的参数在函数形成的闭包中被保存起来。待到函数被真正需要求值的时候，之前传入的所有参数都会被一次性用于求值。




**节流**

我们整理上面提到的三个场景，发现它们面临的共同问题是函数被触发的频率太高。

下面的 throttle 函数的原理是，将即将被执行的函数用setTimeout 延迟一段时间执行。如果该次延迟执行还没有完成，则忽略接下来调用该函数的请求。



### 4章

4.1 单例模式

单例模式是一种常用的模式，有一些对象我们往往只需要一个，比如线程池、全局缓存、浏览器中的 window 对象等。在 JavaScript 开发中，单例模式的用途同样非常广泛。试想一下，当我们单击登录按钮的时候，页面中会出现一个登录浮窗，而这个登录浮窗是唯一的，无论单击多少次登录按钮，这个浮窗都只会被创建一次，那么这个登录浮窗就适合用单例模式来创建。

我们可以用一个变量来判断是否已经创建过登录浮窗，这也是本节第

一段代码中的做法：

```
var createLoginLayer = (function(){ 
   var div; 
   return function(){ 
     if ( !div ){ 
     div = document.createElement( 'div' ); 
     div.innerHTML = '我是登录浮窗'; 
     div.style.display = 'none'; 
     document.body.appendChild( div ); 
   } 
   return div; 
 } 
})(); 

document.getElementById( 'loginBtn' ).onclick = function(){ 
 var loginLayer = createLoginLayer(); 
 loginLayer.style.display = 'block'; 
};
```

通用的单例模式：

```
var getSingle = function( fn ){ 
	var result;
  return function(){
  		return result || ( result = fn .apply(this, arguments ) );
  } 
};

var createLoginLayer = function(){
  var div = document.createElement( 'div' );
  div.innerHTML = '我是登录浮窗';
  div.style.display = 'none'; 2 document.body.appendChild( div );
  return div;
};

var createSingleLoginLayer = getSingle( createLoginLayer );

document.getElementById( 'loginBtn' ).onclick = function(){ 
	var loginLayer = createSingleLoginLayer(); 
	loginLayer.style.display = 'block';
};
```



### 5章 策略模式

策略模式指的是定义一系列的算法，把它们一个个封装起来。将不变的部分和变化的部分隔开是每个设计模式的主题，策略模式也不例外，**策略模式的目的就是将算法的使用与算法的实现分离开来。**

核心：

​	策略和执行分开



定义一系列的算法，把它们各自封装成策略类，算法被封装在策略类内部的方法里。在客户对 Context 发起请求的时候，Context 总是把请求委托给这些策略对象中间的某一个进行计算。

```
var strategies = { 
 "S": function( salary ){ 
 			return salary * 4; 
 }, 
 "A": function( salary ){ 
		 return salary * 3; 
 }, 
 "B": function( salary ){ 
 		return salary * 2;
	}
};
 
var calculateBonus = function( level, salary ){ 
 return strategies[ level ]( salary ); 
}; 
console.log( calculateBonus( 'S', 20000 ) ); // 输出：80000 
console.log( calculateBonus( 'A', 10000 ) ); // 输出：30000

```

5.6.2 用策略模式重构表单校验

策略对象:

```
var strategies = { 
 isNonEmpty: function( value, errorMsg ){ // 不为空
   if ( value === '' ){ 
   		return errorMsg ; 
   } 
 }, 
 minLength: function( value, length, errorMsg ){ // 限制最小长度
   if ( value.length < length ){ 
   		return errorMsg;
   } 
 }, 
 isMobile: function( value, errorMsg ){ // 手机号码格式
   if ( !/(^1[3|5|8][0-9]{9}$)/.test( value ) ){ 
   		return errorMsg; 
   } 
 } 
};
```



### 6章 代理模式

6.3 虚拟代理实现图片预加载

常见的做法是先用一张loading 图片占位，然后用异步的方式加载图片，等图片加载好了再把它填充到 img 节点里，这种场景就很适合使用虚拟代理。

### 8章 发布订阅模式

