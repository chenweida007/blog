10道es6题

1. promise
2. promise.all
3. promist.race
4. proxy
5. reflect






## 1. promise基础知识复习

pending（进行中）、fulfilled（已成功）和rejected

一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。

这是因为立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。



Promise.prototype.catch方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。







## 4. proxy


[proxy的定义](http://es6.ruanyifeng.com/#docs/proxy)



Proxy 可以理解成，在目标对象之前架设一层“拦截”，<font color="red">外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。</font>Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。


Proxy 对象的所有用法，都是上面这种形式，不同的只是handler参数的写法。其中，new Proxy()表示生成一个Proxy实例，target参数表示所要拦截的目标对象，handler参数也是一个对象，用来定制拦截行为。

```
var proxy = new Proxy(target, handler);
```


如果handler没有设置任何拦截，那就等同于直接通向原对象。

```
var target = {};
var handler = {};
var proxy = new Proxy(target, handler);
proxy.a = 'b';
target.a // "b"
上面代码中，handler是一个空对象，没有任何拦截效果，访问proxy就等同于访问target。
```

用自己的话说：  
	可以理解成代理、拦截，对目标对象设置了一层拦截，当我们想访问该对象时，必须通过proxy代理, 对外界的访问进行过滤和改写。es6中通过new proxy生成proxy实例，第一个参数代表要拦截的目标对象，第二个参数也是对象，用来定制拦截行为。
	
	var proxy = new Proxy(target, handler)

支持13种拦截操作，常见的，

+ get 对象属性的读取
+ set 对象属性设置
+ ownKeys  Object.keys  for ..in




## 5. reflect


es6为了操作对象而提供新的api, 

+ 将Object 一些内部方法，放到relect 对象上，Object.defineProperty，部署对象新方法
+ 修改Object 方法的返回结果，更合理
+ Object操作变成函数行为，Reflet.has(obj, key)
+ 与proxy对象方法一一对应，proxy在修改行为的基础上，可以方便的调用reflect方法，完成默认行为




## 6. generator

## 7. module

在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务器，后者用于浏览器。ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

```
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

<font color="red">上面代码的实质是整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”。</font>


import { stat, exists, readFile } from 'fs';
上面代码的实质是从fs模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。


export 可以输出，函数、类、对象

```
export function multiply(x, y) {
  return x * y;
};
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```


<font color="red">import指向 内存地址，导入值会随导出值而变化  
但是导入值不能修改，是只读的</font>


import命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。

import {a} from './xxx.js'

a = {}; // Syntax Error : 'a' is read-only;  
上面代码中，脚本加载了变量a，对其重新赋值就会报错，因为a是一个只读的接口。但是，如果a是一个对象，改写a的属性是允许的。

import {a} from './xxx.js'

a.foo = 'hello'; // 合法操作   
上面代码中，a的属性可以成功改写，并且其他模块也可以读到改写后的值。不过，这种写法很难查错，建议凡是输入的变量，都当作完全只读，不要轻易改变它的属性。