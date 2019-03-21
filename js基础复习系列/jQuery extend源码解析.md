```
jQuery.extend = jQuery.fn.extend = function() { //给jQuery对象和jQuery原型对象都添加了extend扩展方法
  var options, name, src, copy, copyIsArray, clone, target = arguments[0] || {},
  i = 1,
  length = arguments.length,
  deep = false;
  //以上其中的变量：options是一个缓存变量，用来缓存arguments[i]，name是用来接收将要被扩展对象的key，src改变之前target对象上每个key对应的value。
  //copy传入对象上每个key对应的value，copyIsArray判定copy是否为一个数组，clone深拷贝中用来临时存对象或数组的src。

  // 处理深拷贝的情况
  if (typeof target === "boolean") {
    deep = target;
    target = arguments[1] || {};
    //跳过布尔值和目标 
    i++;
  }

  // 控制当target不是object或者function的情况
  if (typeof target !== "object" && !jQuery.isFunction(target)) {
    target = {};
  }

  // 当参数列表长度等于i的时候，扩展jQuery对象自身。
  if (length === i) {
    target = this; --i;
  }
  for (; i < length; i++) {
    if ((options = arguments[i]) != null) {
      // 扩展基础对象
      for (name in options) {
        src = target[name];	
        copy = options[name];

        // 防止永无止境的循环，这里举个例子，
            // 如 var a = {name : b};
            // var b = {name : a}
            // var c = $.extend(a, b);
            // console.log(c);
            // 如果没有这个判断变成可以无限展开的对象
            // 加上这句判断结果是 {name: undefined}
        if (target === copy) {
          continue;
        }
        if (deep && copy && (jQuery.isPlainObject(copy) || (copyIsArray = jQuery.isArray(copy)))) {
          if (copyIsArray) {
            copyIsArray = false;
            clone = src && jQuery.isArray(src) ? src: []; // 如果src存在且是数组的话就让clone副本等于src否则等于空数组。
          } else {
            clone = src && jQuery.isPlainObject(src) ? src: {}; // 如果src存在且是对象的话就让clone副本等于src否则等于空数组。
          }
          // 递归拷贝
          target[name] = jQuery.extend(deep, clone, copy);
        } else if (copy !== undefined) {
          target[name] = copy; // 若原对象存在name属性，则直接覆盖掉；若不存在，则创建新的属性。
        }
      }
    }
  }
  // 返回修改的对象
  return target;
};
```