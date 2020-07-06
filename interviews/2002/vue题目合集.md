####1， vue key 作用

官网推荐使用key, 理解为，使用唯一id 作为key,

更新组件时候，判断两个节点是否相等，相同就复用，不相同就删除旧的，创建新的

	
优点1： 可以复用节点，

只适用于渲染简单无状态组件，



优点2： 可以消除状态缓存，强制更新组件，避免原地复用带来的副作用：

一个新闻列表，点击列表项标记可读未读，tab切换新闻种类，如果没有加key，保留之前的状态，加上新闻id为key, m每次渲染列表都会完全替换所有组件，





缺点： 在某些情况下不带key 效果更好，因为带唯一key每次更新不能找到可复用的节点，不但要销毁和创建vnode，在dom里添加移除节点对性能影响更大。

不带key时候节点能够复用，省去创建销毁组件的开销，只需要修改dom文本内容而不是移除/添加节点


####2， 双向绑定，model 是如何改变 view, view 是如何改变model的？

vue 通过 编译器，将模板文件 vue 编译，生成 模板函数，这个过程中，通过 Object.defineProperty 将 data 数据与 vm 实例绑定起来
render 函数，展示的样子是 with(this)(_c())
访问 vm.name 就是方法 data.name
收集依赖

当我们data变化，触发每个 属性中的 set 函数，会导致 rerender  
而view中数据变化，比如 input ， 监听了 input 内容变化， inputValue = this.value， 从而data变化，又会重新rerender  




####5， 为什么子组件不可以修改父组件传过来的prop

一个父组件下不足有你一个子组件，同样，使用这份prop数据的也不只有你一个子组件。
如果每个子组件都能够修改prop的话，将会导致修改数据的源头不止一处。保证数据修改源唯一。

####6， 为什么 mutation 不能做异步操作？

异步操作是成功还是失败不可预测，什么时候进行异步操作也不可预测

当异步操作成功或失败时，如果不 commit(mutation) 或者 dispatch(action)， vuex 就不能捕获到异步结果从而进行相应操作


####8, Object.defineProperty 有哪些缺陷？		
	
	1. 对于一开始模板中没有使用到的data的属性，不能监听
	2. 无法监控到数组下标变化，无法通过下标给数组设置值
	3. 需要遍历，只能劫持对象的属性，需要对每个对象的每个属性进行遍历
	



####9, 父子组件钩子

加载渲染过程

父 beforeCreate > 父created > 父 beforMount > 子组件beforeCreate > 子组件created > 子组件beforMount >  子组件mounted >  父组件 mounted


子组件更新过程：
父 beforeUpdate > 子beforeUpdate > 子 updated >  父 updated

父组件更新过程：
父 beforeUpdate > 父 updated

销毁过程：

父 beforeDestroy > 子beforeDestroy >子 destroyed > 父destroyed




#### 10， vue 在 v-for 时，给每项元素绑定事件需要使用事件代理吗，为什么

1. 将事件处理程序代理到父节点，减少内存占用率
	
		假设一个div内有100个button，现在需要使每个button点击时alert其序号。如果我们遍历这100个button，给他们一个个绑定上onclick事件，那么浏览器就会针对每个button创建一个引用，这100个引用指向100个匿名函数。在现在的计算机上执行这段代码我可以说并没太大的性能问题，但如果是1000个，10000个button呢？

2. 动态生成子节点能自动绑定事件处理程序到父节点

 
 虽然vue 会让所有 节点都指向同一个事件处理程序，一定程度上比每生成一个节点都绑定一个不同的事件处理程序性能好，但是 js event listener 监听器的数量不会改变，所以使用事件代理会更好。
 
 

### 11. O(n^3)  => O（n）



###12， vue 渲染大量数据如何优化？

1. 前端使用分页
2. 异步渲染组件
3. 使用 v-if 显示可视区域 避免出现大量dom节点
4. 按需加载局部数据，虚拟列表，无限下拉刷新
5. 非响应式数据，Object.freeze 冻结对象，虚拟列表（大量纯展示的数据，不需要追踪变化的，使用Object.freeze 冻结，Object.preventExtension 组织对象扩展来阻止vue给每个对象加上 get、set, 缺点是不能响应。



###13，Vue首屏优化

1. 使用webpack减少打包体积，import 
2. 服务端渲染，在服务端装好首页所需的HTML，使用首页ssr+ 跳转spa方法优化
3. 首页加上loading，骨架屏
4. prefetch, 冷启动
5. 常规操作：cdn、减少请求、雪碧图、Gzip、浏览器缓存、路由懒加载





###15， nextTick 原理

执行栈、执行队列。



回答1： 涉及到js线程中的 宏任务和微任务，在一次事件循环中，先执行微任务，再执行宏任务

宏任务: setTimeOut, setInterVal

微任务： promise, mutationObserver

查询浏览器支持程度，先后执行

1. promise
2. mutationObserve
3. setTimeout

nextTick 好处：碰到太频繁操作js操作，只需要显示最后一次数据的视图，如果每次都更新视图，会消耗太多性能


回答2：vue在默认情况下，每次触发某个数据的 setter 方法后，对应的 watcher 对象其实会被 push 进一个队列 queue 中，在下一个 tick 的时候，将这个队列queue全部拿出来 run 一遍

使用一个 callback 数组 存储 nextTick, 在下一个 tick 前，所有 cb 都被存在这个数组中。




###17， computed 和 watch 有什么区别

计算属性，由data中已知的值，得到一个新值，一个数据受多个数据影响

watch: 监听data中数据的变化，



