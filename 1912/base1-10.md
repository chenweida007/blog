参考文献：  
[面试小册1](https://juejin.im/book/5bdc715fe51d454e755f75ef/section/5bdc715f6fb9a049c15ea4e0)

[import require区别](https://juejin.im/post/5b0685c151882538a808aded
)

[import](https://juejin.im/post/597ec55a51882556a234fcef)

[郭冬冬整理](https://juejin.im/post/5c64d15d6fb9a049d37f9c20#heading-6)

10道基础题

1. 变量提升、暂时性死区
2. 模块化,commonjs与es6区别
3. 手写promise
4. js原型和构造函数之间的关系
5. 闭包、作用域链相关
6. script 引入方式
7. 代码的复用
8. 异步解决方案




## 1. let、const、var

1. 使用 var 声明的变量会被提升到作用域的顶部,let 和 const 不会。   
2. 函数会被提升，并且优先于变量提升。
3. 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。在let声明变量前，对tmp赋值会报错。

let、const:  块级作用域、不存在变量提升、暂时性死区、不允许重复声明  
const: 声明常量，无法修改


## 2. 模块化

#### 1.exoports 和 module.exports 区别？  

在一个node执行一个文件时，会给这个文件内生成一个 exports和module对象，  
而module又有一个exports属性。他们之间的关系如下图，都指向一块{}内存区域。
exports = module.exports = {};

![img](https://user-gold-cdn.xitu.io/2017/7/31/6227d4e0917f4af649d9f9e750eddb09?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

其实require导出的内容是module.exports的指向的内存块内容，并不是exports的。
简而言之，区分他们之间的区别就是 exports 只是 module.exports的引用，辅助后者添加内容用的。

为了避免糊涂，尽量都用 module.exports 导出，然后用require导入。




#### 2. 模块化优点？  

	解决命名冲突  
	提供复用性  
	提高代码可维护性  

ES Module 是原生实现的模块化方案，与 CommonJS 有以下几个区别

##### 1.遵循的规范不同
1. equire/exports是CommonJS的一部分

2. import/export是ES6新规范


#####  2.形式不同

```
export / import : es6
module.exports / exports/ require: commonjs
require / defined: amd

require/exports 的用法只有以下三种：

const fs = require('fs');
 exports.fs = fs;
 module.exports = fs;

import/export的写法就多种多样
import fs from 'fs';
import {default as fs} from 'fs';
import * as fs from 'fs';

export default fs;
export const fs;
export * from 'fs';

```

##### 3.本质上的不同
1. CommonJS还是ES6 Module 输出都可以看成是一个具备多个属性或者方法的对象;  
2. default 是ES6 Module所独有的关键字，export default 输出默认的接口对象，import from 'fs'可直接导入这个对象;  
3. ES6 Module中导入模块的属性或者方法是强绑定的，包括基础类型；而 CommonJS 则是普通的值传递或者引用传递。  



4. require支持 动态导入，import不支持，正在提案 (babel 下可支持)
5. require是 同步 导入，import属于 异步 导入
6. require是 值拷贝，导出值变化不会影响导入值；import指向 内存地址，导入值会随导出值而变化



```
// counter.js
exports.count = 0
setTimeout(function () {
  console.log('increase count to', ++exports.count, 'in counter.js after 500ms')
}, 500)

// commonjs.js
const {count} = require('./counter')
setTimeout(function () {
  console.log('read count after 1000ms in commonjs is', count)
}, 1000)

//es6.js
import {count} from './counter'
setTimeout(function () {
  console.log('read count after 1000ms in es6 is', count)
}, 1000)


0
1

➜  test node commonjs.js
increase count to 1 in counter.js after 500ms
read count after 1000ms in commonjs is 0
➜  test babel-node es6.js
increase count to 1 in counter.js after 500ms
read count after 1000ms in es6 is 1

```



## 3. promise

首先思考。这个方法具有哪些变量和函数？  
状态 
value  
成功回调函数数组  
失败回调函数数组
resove 和 reject 函数干嘛的  
then  







## 4. js原型和构造函数之间的关系

![img](https://user-gold-cdn.xitu.io/2019/2/14/168e9d9b940c4c6f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


![原型关系图](https://user-gold-cdn.xitu.io/2018/11/16/1671d387e4189ec8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```

实例.__proto__ === 原型

原型.constructor === 构造函数

构造函数.prototype === 原型


```
原型链：  

```
目的是什么？共享继承；  
指针是什么？  proto  
最终指向什么，祖先的原型  
属性继承和修改策略 
```

原型链是由原型对象组成，每个对象都有 
``` 
__proto__ 
``` 
</font>属性，指向了创建该对象的构造函数的原型，``` 
__proto__ 
``` 将对象连接起来组成了原型链。是一个用来实现继承和共享属性的有限的对象链。


属性查找机制: 当查找对象的属性时，如果实例对象自身不存在该属性，则沿着原型链往上一级查找，找到时则输出，不存在时，则继续沿着原型链往上一级查找，直至最顶级的原型对象Object.prototype，如还是没找到，则输出undefined；


属性修改机制: 只会修改实例对象本身的属性，如果不存在，则进行添加该属性，如果需要修改原型的属性时，则可以用: b.prototype.x = 2；但是这样会造成所有继承于该对象的实例的属性发生改变。



## 5. 


执行上下文(EC)  
执行上下文可以简单理解为一个对象:  


它包含三个部分:

+ 变量对象(VO)
+ 作用域链(词法作用域)  
+ this指向



它的类型:

+ 全局执行上下文
+ 函数执行上下文
+ eval执行上下文



<font color="red">作用域</font>其实可理解为该上下文中声明的 变量和声明的作用范围。可分为 块级作用域 和 函数作用域


我们知道，我们可以在执行上下文中访问到父级甚至全局的变量，这便是作用域链的功劳。作用域链可以理解为一组对象列表，包含 父级和自身的变量对象，因此我们便能通过作用域链访问到父级里声明的变量或者函数。

由两部分组成:

[[scope]]属性: 指向父级变量对象和作用域链，也就是包含了父级的[[scope]]和AO  
AO: 自身活动对象




闭包的定义可以理解为: 父函数被销毁 的情况下，返回出的子函数的[[scope]]中仍然保留着父级的单变量对象和作用域链，因此可以继续访问到父级的变量对象，这样的函数称为闭包。


## 6. script 引入方式：
html 静态<script>引入
js 动态插入<script>
<script defer>: 延迟加载，元素解析完成后执行
<script async>: 异步加载，但执行时会阻塞元素渲染


##7. 代码的复用
当你发现任何代码开始写第二遍时，就要开始考虑如何复用。一般有以下的方式:

函数封装
继承  
复制extend
混入mixin
借用apply/call


## 8.异步解决方案:


Promise的使用与实现


generator:

yield: 暂停代码
next(): 继续执行代码


```
function* helloWorld() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

const generator = helloWorld();

generator.next()  // { value: 'hello', done: false }

generator.next()  // { value: 'world', done: false }

generator.next()  // { value: 'ending', done: true }

generator.next()  // { value: undefined, done: true }


await / async: 是generator的语法糖， babel中是基于promise实现。

async function getUserByAsync(){
   let user = await fetchUser();
   return user;
}

const user = await getUserByAsync()
console.log(user)
```



