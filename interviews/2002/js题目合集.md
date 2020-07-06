1，["1","2","3"].map(parseInt)

	相当于 
	
	["1","2","3"].map(function(item, index)=> {
		return parseInt(item, index)
	})

3，set map weakset weakmap

	集合，每个值唯一，使用  === 判断两个值是否相等, add，delete，has,clear
	字典，key可以是object， map 键值是跟内存地址绑定的，get，set,has, delete, clear
	weakset，key只能 存储对象引用
	weakmap，key只能 存储对象引用

6，iife, 立即执行函数，防止变量宠物  
	amd, require.js，依赖前置，  
	cmd, seajs, 依赖就近，使用之前才使用，使用之前再 require   
	common.js, node.js 中使用的规则，require， exports, module.exports  
	umd, amd 和 commonjs 兼容  
	es, import  from, export default  

7，const 和 let 声明变量不在Window上
	
	通过{} 生成块级作用域， 在方法的[[Scopes]] 属性中，看到变量a
	
8， 函数表达式不能 变量提升，函数声明可以
9，匿名函数的执行是具有全局性的，那怎么具有的全局性呢？
this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象

```
var b = 10;
(function b(){
    b = 20;
    console.log(this.b);

})();
```

12， 有 length 和 slice 可以将类数组转为数组

```
var obj = {
    '2':3,
    '3':4,
    'length': 2,
    'splice': Array.prototype.splice,
    'push':Array.prototype.push
}
obj.push(1)  // obj[2] = 1， length的位置开始添加数据
obj.push(2)  // obj[3] = 2,
console.log(obj)


{
	empty × 2,
	2: 1
	3: 2
	length: 4
	splice: ƒ splice()
	push: ƒ push()
}
```

15，箭头函数

函数体内的this是定义时所在的对象，不是使用时的对象    
不能使用arguments对象    
不能使用yield明星，不能作为 generator 函数    
不可以使用new  
没有自己的this, 不能使用 call\apply  
没有prototype属性


16, a.b.c.d 比 a[b][c][d]效率高  

后者还要考虑变量问题

17，es6转为es5 

1. 使用babel/parser  字符代码串解析生抽象语法树，ast，
2. 对ast进行处理，对es6代码进行相应转为，转成es5, 如 let const 转为 var, 箭头函数转为 function 等
3. 采用 babel-polyfill 对 es5 中不存在的 api 做修复，使用相应的es5代码实现这些api, 如 map、 set
4. 处理后的ast再生出代码字符串  


18， for 效率高于 forEach


20， 普通对象，key 默认是字符串

任何一个 symbol 对象值都是不相等的

```
a = {}, c = 123, a[c] = 'c' 

a['123'] = 'c'


```

21， input 搜索防抖，处理中文输入

23， 前端使用加密的场景


1，密码加密,
2，传输内容加密， https
3，展示内容加密
	展示的的是， abc， 加密后 对应的 数据是 edf, web 反爬虫


25，

实例上有两个a方法，哪个优先级高？
一个是内部属性方法，另一个是原型方法，当两者重名时候，前者优先级高

Foo 函数必须初始化后，才能挂到全局变量上去


26，

```
String('11')  // typeof string('11')  'string'
String('11') == new String('11')  // 隐式转换，toString， true
String('11') === new String('11')  // string， object  ，false
```


27, var 会被提升到 function 内部的 顶部，

28，全局变量，不会变量提升，如果没找到，直接去外层作用域找

29，[1,2]+ [2,3]  // toString, 1,223
'a' + + 'b'  // 'aNaN'


30， 为什么 for 循环嵌套顺序会影响性能？


外层越大，越影响性能

i，判断100次  
j，判断100* 1000次  
k，判断 100 * 1000 * 10000 次  


31， 执行机制

promise相当于同步任务，会立即执行，then后面的是微任务
setTimeout, 异步任务



