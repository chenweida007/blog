vue组件间通信方式

1.方法一、 props　/ $emit
父组件A通过props的方式向子组件B传递，B to A 通过在 B 组件中 $emit, A 组件中 v-on 的方式实现。


```
<h1 @click="changeTitle">{{title}}</h1>//绑定一个点击事件


```