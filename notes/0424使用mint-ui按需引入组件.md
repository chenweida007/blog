
## 需要使用 mint-ui  switch 等组件

引入mint-ui，但是不想全部引用，按需引入

[官方文档之快速入手篇](http://mint-ui.github.io/docs/#/zh-cn/quickstart)

记录下爬坑之旅：

``` js

1. npm install mint-ui  --save  

2. npm install babel-plugin-component  //需要安装这个插件

3. 修改 babel.config.js 文件

module.exports = {
  presets: [
    '@vue/app'
  ],
  plugins:[
      [
        "component",
        {
          "libraryName": "mint-ui",
          "style": true
        }
      ]
  ]
}

如果已经有值，在后面追加配置即可

module.exports = {
  presets: [
    '@vue/app'
  ],
  plugins: [
    "transform-vue-jsx",
    "transform-runtime",
    ["component", [
      {
        "libraryName":"mint-ui",
        "style":true
      }
      ]
    ]
  ]
}

4. 在main.js 中添加 引入组件的配置

需要什么组件，去mint-ui，src下面的index.js 中找，默认会找这个文件,
看导出哪些组件名，需要就拿过来用

import { Switch, Range } from 'mint-ui' 
import 'mint-ui/lib/style.css'

Vue.component(Switch.name, Switch)
Vue.component(Range.name, Range)     //Vue.use 不行，抛弃

成功大吉！

```

顺便记录下全部引入

```js
1. npm intall mint-ui  --save  

2. 在main.js 中添加值

import Mint from 'mint-ui';
import 'mint-ui/lib/style.css'
Vue.use(Mint);


```


最后呈现的样子

``` 
//main.js

import { Switch } from 'mint-ui'
import 'mint-ui/lib/style.css'
Vue.use(Switch)
Vue.component(Switch.name, Switch)


// vue 文件中
<template>
  <div id="app">
		<mt-switch v-model="value" @change="turn">Switch</mt-switch>
  </div>
</template>

//methods
turn 事件，会自动改变 this.value 
不需要对this.value = ! this.value


不需要再 import switch 组件，也不需要在 component中注册了，在 main.js中已经做了

```




## 参考文章

1. https://www.cnblogs.com/holdon521/p/9548894.html
2. http://www.pianshen.com/article/453256461/
3. https://blog.csdn.net/qq_42484025/article/details/88719733


## 注意点

 部分引入使用 Vue.use 不行， but don't know why
 












