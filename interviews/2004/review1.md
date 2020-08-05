如果没有vuex会怎么做



用正则获取URL中参数  



如果不用 vuex 全局数据，管理你会怎么做？

组件间传值方式



mpvue 原理

并发6个请求，限制如何做到？

如果限制一分钟限制10个请求？



qscode 原理

二维码本质？

为什么要使用 map、set



4.08.02

用户快速滑动下一张图片，怎么处理

如果微博首页，js文件完全丢失，如何操作？



4.13

```
$("p:first").addClass("intro");

let div = document.querySelector('div');
div.classList.add("newClass");
div.classList.remove("newClass");

```

具体哪个元素点击的：

```
x=event.target; 

x.id

<p id="p1" onmousedown="getEventTrigger(event)">
Click on this paragraph. An alert box will show which element triggered the event.</p>
```

第一个div元素

```
div:nth-of-type(1)
```

4.14

性能优化，如果没有使用 webpack 如何做？
给我一个checklist

讲一讲 DNS 查找 IP 

disabled 和  readonly 有什么区别

```html
<html>
<div class="row" id="product-plan">
    <div class="col-xs-10 col-sm-10 col-md-10 col-lg-10">
        <ul  onclick="getclick(event)" id="0">
            <li id="1">版式文件产品</li>
            <li id="2">基于位置服务</li>
            <li id="3">司法信息化解决方案</li>
            <li id="4">版式应用解决方</li>

        </ul>
    </div>
</div>
<script>
function getclick(e) {
    console.log(e.currentTarget.id)
    console.log(e.target.id)
}

</script>
</html>
```