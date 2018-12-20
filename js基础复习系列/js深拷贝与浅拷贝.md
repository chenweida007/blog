

## 先问自己几个问题
1. 什么是深拷贝？什么是浅拷贝？
2. 如何实现深拷贝？
3. 递归停止的条件是什么？
4. $.extend() 和 Object.assign() 什么区别？
5. 如何用上面两个实现深拷贝？可以实现深拷贝么？

## 为啥要写

这个问题几乎是烂大街了，基本面试都会遇到，网上写的深拷贝浅拷贝文章也非常多，但还是要写，为啥，因为总感觉别人写的思路和自己不太一样，然后有些问题没写清楚自己也不懂，就是按照自己的思路写吧，希望和大家交流，哪里写错了希望大家指出



## 思考
### 1.基本类型和引用类型区别
基本类型保存在栈中，保存的是值  
引用类型保存在堆中，栈中存放的是数据在堆中的地址，使用的时候，我们先从栈中取出地址，然后根据地址去堆中找相应的数据。    

### 2.浅拷贝和深拷贝区别
名字联想，浅拷贝就是，浅显简单拷贝，但是会有问题  
什么问题？拷贝基本类型就是拷贝值，改A，B不会跟着变， 但涉及到拷贝引用类型的数据时，拷贝的是数据的地址，修改原数据时候，拷贝的数据也会跟着修改  

深拷贝，递归拷贝，所有拷贝都是拷贝值，没有拷贝地址，你变，我不变

### 3.结论
#### 3.1 浅拷贝方法：
	Array.prototype.slice();
	Array.prototype.concat();
	Object.assign;
	直接拷贝
	
	




Object.assign




### 4.深拷贝的几种方式
  ● 
  ● 如果你的对象里有函数,函数无法被拷贝下来
  ● 无法拷贝copyObj对象原型链上的属性和方法


var a = { n: {name:'whatever'} }; 
var b = JSON.parse( JSON.stringify(a) );

  ● 用jQ实现深拷贝   // $.extend ({}, obj)  的实现

$.extend( [deep ], target, object1 [, objectN ] )

递归后的，赋值，没有考虑fucntion ?



var obj1 = { a: 1, b: { c: 2 } };
var obj2 = Object.assign({}, obj1);
obj2.a = 3;
obj2.b.c = 3;
console.log(obj1); // { a: 1, b: { c: 3 } }
console.log(obj2); // { a: 3, b: { c: 3 } }


参考文章：
掘金：
https://juejin.im/post/5b5dcf8351882519790c9a2e


segmentfault:




## 参考资料
[参考一](https://juejin.im/post/5beb93de6fb9a049c30ac9ee)  
[参考二](https://juejin.im/post/5b5dcf8351882519790c9a2e)



