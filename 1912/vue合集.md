vue组件间通信方式



1.方法一、 props　/ $emit
父组件A通过props的方式向子组件B传递，B to A 通过在 B 组件中 $emit, A 组件中 v-on 的方式实现。


```
<h1 @click="changeTitle">{{title}}</h1>//绑定一个点击事件


changeTitle(){
      this.$emit("titleChanged","子向父组件传值");//自定义事件  传递值“子向父组件传值”
    }

```



2. 方法2，vuex

state管理状态
mutation修改状态
action异步修改状态


3. 几个兄弟模板之间，可以使用这种方法，几个模板都能访问到这个event对象，
var Event = new Vue ();
    
Event.$emit(事件名,数据);
    
Event.$on(事件名,data =>{});





参考文章：

https://mp.weixin.qq.com/s/vB_9Q4DyqE5crO-hpD_kWg