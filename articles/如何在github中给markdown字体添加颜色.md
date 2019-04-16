## 问题

最近看一群大佬们发文章啊，github提issue啊，简直是热火朝天，轮到我自己时候  

咦，我在本地编辑的md 文件，可以给文字添加颜色

怎么放到github中颜色就自动消失了？


## Google一下

原来已经有不少人提出这个问题了，我们来看看大家的解决办法

我想给我的 README.md 文档添加颜色标签，  
我尝试 使用 以下两种方式，但是效果不行

``` js
<span style="color: green"> Some green text </span>

And this:

<font color="green"> Some green text </font>


```

解决方案1：

值得一提的是，您可以使用占位符图像服务在自述文件中添加一些颜色。
例如，提供颜色列表以供参考：


- ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `#f03c15`
- ![#c5f015](https://placehold.it/15/c5f015/000000?text=+) `#c5f015`
- ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `#1589F0`


解决方案2：

可以使用diff tag 来生成 高亮文本。

```diff
- red
+ green
! orange
# gray
```


解决方案3：  

github目前不支持文档color、 style标签  

如果你这样写了 

<span style="color:orange;">Word up</span>

github会视为html

虽然不支持添加 color tag ，但是我们可以使用以下tag去添加一些颜色

```json
   // code for coloring
```
```html
   // code for coloring
```
```js
   // code for coloring
```
```css
   // code for coloring
```
// etc.


如果你真的非常想在readme.md 中使用颜色，可以在readme.md中链接到 readme.html  文件，这个文件里可以使用样式呀

3. 



## 总结

1. 使用 diff + - ! #（可以！满足基本需求
2. 文字加不了颜色，给这一行加上有颜色的表头图片 (麻烦，pass
3. 使用 ``` js、```html、 ```css （虽然不是想要的，pass
4. 链接到 readme.html 文件（也不太好


> github写作格式语法

[Writing on GitHub](https://help.github.com/en/categories/writing-on-github)   
[Basic writing and formatting syntax](https://help.github.com/en/articles/basic-writing-and-formatting-syntax)



## 参考链接

1. [有没有办法在Github Flavored Markdown中获得彩色文字](http://landcareweb.com/questions/3231/you-mei-you-ban-fa-zai-github-flavored-markdownzhong-huo-de-cai-se-wen-zi-zhong-fu)
2. [How to add color to Github's README.md file](https://stackoverflow.com/questions/11509830/how-to-add-color-to-githubs-readme-md-file)
3. [No longer possible to color text in Markdown](https://github.com/github/markup/issues/369)  
4. [Github中的Markdown支持用不同的颜色显示文字吗](https://github.com/guodongxiaren/README/issues/21)