10道es6题

1. promise
2. promise.all
3. promist.race
4. proxy






## 1. promise基础知识复习

pending（进行中）、fulfilled（已成功）和rejected

一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。

这是因为立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。



Promise.prototype.catch方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。