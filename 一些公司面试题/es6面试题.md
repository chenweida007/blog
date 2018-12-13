每天搞定一点点！加油！

### es6面试题
es6面试题一览

### 自己整理的题目+答案

1.es5中的this和es6中的this区别  

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
先用that把this保存起来  
或者使用 bind(this)
bind(this)的原理？





### 网上搜集的es6题
[面试题一](https://blog.csdn.net/nimeghbia/article/details/80733418)  
[面试题二](https://www.cnblogs.com/xuepei/p/8855913.html)  
[面试题三](https://blog.csdn.net/sinat_36174237/article/details/82378891)  
[面试题四](https://www.cnblogs.com/fengxiongZz/p/8191503.html)  
[面试题五](https://segmentfault.com/a/1190000011344301)  
[面试题六](http://www.bslxx.com/a/mianshiti/tiku/javascript/2017/1213/1505.html)  
[面试题七](https://segmentfault.com/a/1190000016848192)

