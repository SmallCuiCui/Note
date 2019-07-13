# flex布局

## 容器盒子上的属性

### 子项目主轴方向

```
flex-direction（方向）：{
	row  //默认水平从左向右
	row-reverse // 主轴水平从右向左
	column // 主轴纵向从上往下
	column-reverse // 主轴纵向从下往上
}
```

### 子项目排列规则

```
flex-wrap:{
    nowrap（默认）//不换行。
    wrap //从左往右排列，换行
    wrap-reverse //从底部开始从左往右向上排列
}
```

### 子项目在主轴上的排列方式

```
justify（齐行）-content（内容）:{
    flex-start //从左往右排列
    flex-end //从右向左排列
    center //居中
    space-between //两端对齐，项目之间的间隔都相等。
    space-around //每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
}
```

### 子项目在交叉轴上的对齐方式

``` 
align（排列）-items（项目）:{
    flex-start：主轴的起点对齐。
    flex-end：主轴的终点对齐。
    center：主轴的中点对齐。
    baseline: 项目的第一行文字的基线对齐。
    stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
}
```

### 多行子项目排列方式

```
*当有多个子项目并且换行，有多条主轴时才生效。
align-content:{
	flex-start：主轴的起点对齐。
    flex-end：主轴的终点对齐。
    center：主轴的中点对齐。
    space-between：主轴两端对齐，轴线之间的间隔平均分布。
    space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
    stretch（默认值）：轴线占满整个交叉轴。
}
```

## 子项目上的属性

```
order: Number 
// 设置的值越大，在容器内位置越靠前。默认值为0
*一般用于圣杯布局，优先加载中间

flex-grow（扩大）：Number 
// 设置的值越大，分得的剩余空间大小越多。
例：
div1：flex-grow：1
div2: flex-grow：2 
如果有剩余空间。div2分得的剩余空间比div1大一倍。

flex-shrink（收缩）：Number 
// 默认值为1，如果不换行，设置该属性可固定子项目是否被压缩。

flex：（flex-grow, [flex-shrink , flex-basis]）
//简写方式。默认值为0 1 auto后两个属性可选。

flex-basis（基础）
//属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

align-self：{
	auto | flex-start | flex-end | center | baseline | stretch;
}
// 单独设置侧轴排列方式。
```

## 应用

```
<div class="box">
	<div class="div">
				
	</div>
</div>

.box{
	width: 400px;
    height: 400px;
    border: 1px solid red;
    display: flex;
    justify-content: center;
			}
			.div{
    width: 100px;
    height: 100px;
    background-color: aqua;
    align-self: center;
}

*面试题，元素如果水平垂直居中
```

## 进阶

骰子布局

<http://www.ruanyifeng.com/blog/2015/07/flex-examples.html>