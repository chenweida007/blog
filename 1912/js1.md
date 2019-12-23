10道js题  

1. 防抖节流
2. bind、call、apply
3. 深拷贝
4. new





## 1. 防抖和节流

```
// 防抖
function debounce(fn, wait) {    
    var timeout = null;    
    return function() {        
        if(timeout !== null)   clearTimeout(timeout);        
        timeout = setTimeout(fn, wait);    
    }
}
// 处理函数
function handle() {    
    console.log(Math.random()); 
}
// 滚动事件
window.addEventListener('scroll', debounce(handle, 1000));




var throttle = function(func, delay) {            
　　var prev = Date.now();            
　　return function() {                
　　　　var context = this;                
　　　　var args = arguments;                
　　　　var now = Date.now();                
　　　　if (now - prev >= delay) {                    
　　　　　　func.apply(context, args);                    
　　　　　　prev = Date.now();                
　　　　}            
　　}        
}        
function handle() {            
　　console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(handle, 1000));



// 节流throttle代码（定时器）：
var throttle = function(func, delay) {            
    var timer = null;            
    return function() {                
        var context = this;               
        var args = arguments;                
        if (!timer) {                    
            timer = setTimeout(function() {                        
                func.apply(context, args);                        
                timer = null;                    
            }, delay);                
        }            
    }        
}        
function handle() {            
    console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(handle, 1000));


```



函数防抖：将几次操作合并为一此操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。






函数节流：使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。


区别： 函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。<font color="red"> 比如在页面的无限加载场景下，我们需要用户在滚动页面时，每隔一段时间发一次 Ajax 请求，而不是在用户停下滚动页面操作时才去请求数据。</font>这样的场景，就适合用节流技术来实现。


所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。  
所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。节流会稀释函数的执行频率

用一句话总结防抖和节流的区别：防抖是将多次执行变为最后一次执行，节流是将多次执行变为每隔一段时间执行



[面试之道](https://juejin.im/book/5bdc715fe51d454e755f75ef/section/5c0678636fb9a049b347c0aa)

考虑一个场景，滚动事件中会发起网络请求，但是我们并不希望用户在滚动过程中一直发起请求，而是隔一段时间发起一次，对于这种情况我们就可以使用节流。

考虑一个场景，有一个按钮点击会触发网络请求，但是我们并不希望每次点击都发起网络请求，而是当用户点击按钮一段时间后没有再次点击的情况才去发起网络请求，对于这种情况我们就可以使用防抖。

理解了防抖的用途，我们就来实现下这个函数







[参考1](https://www.cnblogs.com/momo798/p/9177767.html)


[参考2](https://www.cnblogs.com/momo798/p/9177767.html)


[参考3](https://www.jianshu.com/p/c8b86b09daf0)

https://www.cnblogs.com/youma/p/10559331.html



##1.排序算法


冒泡算法

```
function bubble(array) {
  checkArray(array);
  for (let i = array.length - 1; i > 0; i--) {
    // 从 0 到 `length - 1` 遍历
    for (let j = 0; j < i; j++) {
      if (array[j] > array[j + 1]) swap(array, j, j + 1)
    }
  }
  return array;
}


插入排序，第一个是有序的
function insertion(array) {
  if (!checkArray(array)) return
  for (let i = 1; i < array.length; i++) {
    for (let j = i - 1; j >= 0 && array[j] > array[j + 1]; j--)
      swap(array, j, j + 1);
  }
  return array;
}


每次选出最小的数，

function selection(array) {
  if (!checkArray(array)) return
  for (let i = 0; i < array.length - 1; i++) {
    let minIndex = i;
    for (let j = i + 1; j < array.length; j++) {
      minIndex = array[j] < array[minIndex] ? j : minIndex;
    }
    swap(array, i, minIndex);
  }
  return array;
}




function jsQuickSort(array) {
    if (array.length <= 1) {
        return array;
    }
    const pivotIndex = Math.floor(array.length / 2);
    const pivot = array.splice(pivotIndex, 1)[0];  //从数组中取出我们的"基准"元素
    const left = [], right = [];
    array.forEach(item => {
        if (item < pivot) {  //left 存放比 pivot 小的元素
            left.push(item); 
        } else {  //right 存放大于或等于 pivot 的元素
            right.push(item);
        }
    });
    //至此，我们将数组分成了left和right两个部分
    return jsQuickSort(left).concat(pivot, jsQuickSort(right));  //分而治之
}

const arr = [98, 42, 25, 54, 15, 3, 25, 72, 41, 10, 121];
console.log(jsQuickSort(arr));  //输出：[ 3, 10, 15, 25, 25, 41, 42, 54, 72, 98, 121 ]

```



## 2. bind\apply、bind



## 继承

```

组合继承
function Parent(value) {
  this.val = value
}
Parent.prototype.getValue = function() {
  console.log(this.val)
}



//调用父类的构造函数

function Child(value) {
  Parent.call(this, value)
}
Child.prototype = new Parent();
Child.prototype.constructor = Child;


// es6继承

class Parent {
  constructor(value) {
    this.val = value
  }
  getValue() {
    console.log(this.val)
  }
}
class Child extends Parent {
  constructor(value) {
    super(value)
  }
}
let child = new Child(1)
child.getValue() // 1
child instanceof Parent // true



1, 原型链继承

function Animal() {
    this.type = 'Cat'
    this.name = 'Nini'
    this.hobbies = ['eat fish', 'play ball']
}
Animal.prototype.say = function () {
    console.log('type is ' + this.type + ' name is ' + this.name);
}

function Cat() {
    this.age = '1'
}
Cat.prototype = new Animal()
Cat.prototype.constructor = Cat


```

### 深拷贝

```
function deepClone(obj) {
    var result = Array.isArray(obj) ? [] : {};
    for (var key in obj) {
      if (obj.hasOwnProperty(key)) {
        if (typeof obj[key] === 'object' && obj[key]!==null) {
          result[key] = deepClone(obj[key]); 
        } else {
          result[key] = obj[key];
        }
      }
    }
    return result;
  }

```

## call

```


Function.prototype.myCall = function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  context = context || window  //首先 context 为可选参数，如果不传的话默认上下文为 window
  
  //其实是为了找到 fn, this 就是 fn, fn要挂载到 context上
  context.fn = this
  const args = [...arguments].slice(1)
  const result = context.fn(...args)
  delete context.fn
  return result
}

```

## apply 


```
Function.prototype.myApply = function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  context = context || window
  context.fn = this
  let result
  // 处理参数和 call 有区别
  
  
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }
  delete context.fn
  return result
}
```



## bind

```
//f.bind(obj, 1)(2) 

Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  const _this = this
  const args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}


//ES6 + 写好的myApply
Function.prototype.myBind = function(obj,...arg1){   //arg1收集剩余参数
    return (...arg2) => {   //返回箭头函数, this绑定调用这个方法(myFind)的函数对象
        return this.myApply( obj, arg1.concat(arg2) );   // 将参数合并
    }
}



ag2是第二个参数列表
// 其实没什么说的
Function.prototype.myBind = function(obj,...arg1){
    return (...arg2) => { 
        let args = arg1.concat(arg2);
        let val ;
        obj._fn_ = this;
        val = obj._fn_( ...args ); 
        delete obj._fn_;
        return val
    }
}
```











## new 

```
function newParent(){
    var obj = {}; // 首先创建一个对象
    obj.__proto__ = Parent.prototype; // 然后将该对象的__proto__属性指向构造函数的protoType
    var result = Parent.call(obj) // 执行构造函数的方法，将obj作为this传入
    return typeof(result) == 'object' ?  result : obj
}
```

var a = new PAreen();


[参考文章1](https://juejin.im/post/5d9eef20e51d45781332e961)


第一遍写：  
防抖和节流的参数是什么？fn的参数？？
fn.call(that, )

没有 pre = Date.now();









