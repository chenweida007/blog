## 先问自己几个问题
1. js数据类型有哪些？
2. 如何判断数据类型
3. instanceOf Object.prototype.toString typeof 局限性 区别 怎么用
4. 


## 思考
### 1. js数据类型
   分为基本数据类型和引用数据类型  
	基本数据类型有6种，boolean string number undefinded null symbol  
	引用类型，Function Array Object  
	
### 2. 如何判断数据类型
记得的哪几种方法是用来判断数据类型的？  
instanceOf typeof Object.prototype.toString

分别怎么用？每个方法有啥局限性？

#### 2.1 typeof
使用：基本数据类型能判断出  
局限性：不能区分Array和Object  

```js
// 基本类型
typeof 123;  //number
typeof "123"; //string
typeof true; //boolean
typeof undefined; //undefined
typeof null; //object
let s = Symbol;
typeof s; //symbol

// 引用类型
typeof [1,2,3]; //object
typeof {}; //object
typeof function(){}; //function
typeof  Array; //function  Array类型的构造函数
typeof Object; //function  Object类型的构造函数
typeof Symbol; //function  Symbol类型的构造函数
typeof Number; //function  Number类型的构造函数
typeof String; //function  String类型的构造函数
typeof Boolean; //function  Boolean类型的构造函数

```

2.2 instanceOf

2.3 Object.prototype.toString.call  

作用：可以识别所有数据类型  
封装的函数：

```
function getType(obj) {
    return Object.prototype.toString.call(obj).slice(8,-1);
}

var a = [1,2,3];
console.log(getType(a)); //Array 

var b = function(){};
console.log(getType(b)); //Function
```






参考文章：
掘金：
https://juejin.im/post/5b5dcf8351882519790c9a2e


segmentfault:




## 参考资料
[参考一](https://juejin.im/post/5beb93de6fb9a049c30ac9ee)  
[参考二](https://juejin.im/post/5b5dcf8351882519790c9a2e)

[三](https://juejin.im/post/5b304fbef265da59a76c94a6)

