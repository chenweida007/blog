### 探探面试问题

1.数组和链表区别      
2.原型链  
3.三角形  
4.http,httpS  
5.防抖和节流  
6.instanceOf  
7.几个题，输出结果  
8.promie.all  



9.全局变量，区别？


	在浏览器里，非严格模式下(印象中必须是这个)，global等于window。
	所以global对象指的是什么，取决于运行环境。更像是个抽象概念，window就很具体了，就是浏览器的一个web api
	global 是 javascript 运行时所在宿主环境提供的全局对象，是一个 Object。目前来说最常见的宿主环境是浏览器和 nodejs，
	
	浏览器里面 window 就是 global
	nodejs 里没有 window，但是有个叫 global 的



10.类

```
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};


class Point {
}

class ColorPoint extends Point {
}
```






小程序1  
https://github.com/shipskunkun/album-review/blob/master/article/1.md


小程序2  
https://github.com/shipskunkun/videoplayer-review/blob/master/article/player1.md

视频滑动项目  
https://github.com/shipskunkun/video-swiper/blob/master/article/videoswiper.md


个人简历  
https://github.com/shipskunkun/album-review/blob/master/jianli.pdf