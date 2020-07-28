## css 部分

1. 选择器 + > ~     + 相邻选择器， > 后代选择器，~ 兄弟选择器

2. less 比 css 好在哪里
	
	 	变量、函数、计算、嵌套、mixin

	1. 变量, @width: 10px;
	2. 函数
	
	3. mixin		
		
		混合（Mixin）是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法。假设我们定义了一个类（class）如下：
		
		```
		.bordered {
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}
		
		#menu a {
		  color: #111;
		  .bordered();
		}
		```
	4. 嵌套
		
			用 Less 书写的代码更加简洁，并且模仿了 HTML 的组织结构。
	5. 	运算（Operations）
		
			@conversion-1: 5cm + 10mm; // 结果是 6cm
			
			calc() 特例
	
	
			为了与 CSS 保持兼容，calc() 并不对数学表达式进行计算，但是在嵌套函数中会计算变量和数学公式的值。
			
			@var: 50vh/2;
			width: calc(50% + (@var - 20px));  // 结果是 calc(50% + (25vh - 20px))
	
3. position
	
		relative、static、absolute、fixed、inherit
4. relative 脱离文档流了没有
	
		relative	否
5. relative 和 position 区别
	
		相对于自身，并不脱离文档流，不管你怎么移动，它原有的位置还是会留着。
	相对于父元素不是static， 并且脱离文档流
6. flex 和 grid 区别	
	
		flex是一维布局 ，grid是二维布局也就是说grid布局可以更好的操作行和列。
	flex布局和grid布局是现在的主流的两种布局方式。
7. 介绍一下flex
	
		display
		flex-direction
		flex-wrap
		align-items
		justify-content
		flex: flex-grow flex-shrink flex-basis
		flex: 0 1 auto
8. 哪些布局方式		

		浮动
		flex
		grid
		position
		table