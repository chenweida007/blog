## 2章 this、call、apply

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


## 3章

变量的生存周期：

而对于在函数内用 var 关键字声明的局部变量来说，当退出函数时，这些局部变量即失去了它们的价值，它们都会随着函数调用的结束而被销毁:


被定义时：

这是因为当执行 ```var f = func();```时，f 返回了一个匿名函数的引用，它可以访问到 func() 被调用时产生的环境，而局部变量 a 一直处在这个环境里。既然局部变量所在的环境还能被外界 访问，这个局部变量就有了不被销毁的理由。在这里产生了一个闭包结构，局部变量的生命看起来被延续了。

闭包：

	 封装变量
	 
	 延续局部变量的寿命

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


局部变量本来应该在函数退出的时候被解除引用，但如果局部变量被封闭在闭包形成的环境 中，那么这个局部变量就能一直生存下去。从这个意义上看，闭包的确会使一些数据无法被及时 销毁。

使用闭包的一部分原因是我们选择主动把一些变量封闭在闭包中，因为可能在以后还需要使用这些变量，把这些变量放在闭包中和放在全局作用域，对内存方面的影响是一致的，这里并 不能说成是内存泄露。如果在将来需要回收这些变量，我们可以手动把这些变量设为 null。

跟闭包和内存泄露有关系的地方是，使用闭包的同时比较容易形成循环引用，如果闭包的作用域链中保存着一些 DOM 节点，这时候就有可能造成内存泄露。但这本身并非闭包的问题，也并非 JavaScript 的问题。

在 IE 浏览器中，由于 BOM 和 DOM 中的对象是使用 C++以 COM 对象 的方式实现的，而 COM 对象的垃圾收集机制采用的是引用计数策略。在基于引用计数策略的垃 圾回收机制中，如果两个对象之间形成了循环引用，那么这两个对象都无法被回收，但循环引用造成的内存泄露在本质上也不是闭包造成的。

同样，如果要解决循环引用带来的内存泄露问题，我们只需要把循环引用中的变量设为 null 即可。将变量设置为 null 意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运 行时，就会删除这些值并回收它们占用的内存。


**函数柯里化**

currying 又称部分求值。一个 currying 的函数首先会接受一些参数，接受了这些参数之后， 该函数并不会立即求值，而是继续返回另外一个函数，刚才传入的参数在函数形成的闭包中被保存起来。待到函数被真正需要求值的时候，之前传入的所有参数都会被一次性用于求值。


我们常常让类数组对象去借用 Array.prototype 的方法，这是 call 和 apply 最常见的应用场景之一:

```
(function(){
	Array.prototype.push.call( arguments, 4 ); // arguments 借用 Array.prototype.push 方法 console.log( arguments ); // 输出:[1, 2, 3, 4]
})( 1, 2, 3 );
```