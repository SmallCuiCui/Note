### 浮动布局
#### 浮动问题：高度塌陷

需求：在实际网页开发中，需要容器高度自适应，父元素不设置高度，让子元素撑开，当子元素使用浮动，会发生高度塌陷
（在标准流中子元素可以把父元素撑开，浮动流中不能把容器撑开）

#### 解决浮动问题

清除浮动-即清除浮动的负面影响（高度塌陷问题）
清除浮动的方法：

1. overflow:hidden;给父元素设置   
   * 原来作用：文本溢出隐藏 以及隐藏元素   
   * 清除浮动带来的影响
2. 使用空盒子方法 在需要清除浮动的元素同级下加一个空元素（该元素不能有其他任何样式）设置clear:both
3. 出现高度塌陷时才清除浮动，其他时候不需要

程序中，数字和字母在一行装不下时不能自动换行，英文单词（区别于字母有空格）及文字可以自动换行

同一文本大小，字体不一样，呈现效果不一样

### css属性

#### 字体font相关

* font-family

  字体类型,谷歌默认微软雅黑，字体可以设置多个，采用逗号隔开，从第一个开始解析，有就采用，没有就继续解析，都没有就采用默认
  字体是中文或含有空格的英文时需要用引号，单个单词不需要加引号   font-family:"微软雅黑","黑体","Times New Romam","Arail";

* font-size

  字体大小  浏览器中默认大小为16px  谷歌中默认支持最小为12px  ；1pt=0.75px   

* 文字加粗 

  font-weight: normal(常规),bolder(更粗),bold(加粗),数值(数值时没有单位)100-400一般，500常规，600-900加粗字体，九个等级

* 文字倾斜 

  font-style:italic(斜体字)/oblique(倾斜)/normal(常规)

* 文字行高  line-height:normal/value
  单行文本的行高等于容器高时，实现文本垂直居中对齐

* font缩写  font:14px/24px "黑体";==font-size:12px;line-height:24px;font-family:"黑体";   必须字号与字体同时设置才有效,font-size与line-height需用斜线隔开
  顺序：font-style, font-weight,font-size/line-height,font-family

文本修饰  text-decoration:underline/overline(上划线)/line-through(删除线)/none(没有下划线);

首行缩进  text-indent:2em;相对于父元素缩进两个字符


列表符号属性list-style  
?	list-style-type:none/circle(空心圆)/square(实心方块)/disc(实心圆);
?	list-style-image:url();图片作为列表符号 列表符号的位置不好调整,常采用背景图片取代
?	list-style-position:inside/outside(默认值)

背景图固定:background-attachment:fixed(固定)/scroll(滚动);
?	默认显示：1背景图大于容器，部分显示不下，小于容器时会平铺

border-width:边框宽
border-style:dotted(点)/dashed(虚线)/double(双线)  要有双线border-width大于等于3

宽屏布局*//窄屏布局
宽屏页面怎么写：1、要适应不同电脑的分辨率
?	2、开发中宽屏的页面不能出现横向的滚动条
?	采用外层适应百分比，内层使用固定布局形式-版心
?	流式布局方式：采用百分比的布局方式

标准流下块级元素特性：宽度默认充满容器
?	块级元素不设置宽和设置了宽100%效果上是一样的，区别是一个有宽一个没有宽，当脱离文档流之后没有设置宽的元素的宽为0，脱离文档流的元素需要加上宽

当给子元素设置margin-top时，浏览器在解析代码时会误认为给父元素设置的，所以整体会往下的情况。
?	注意点：1、子元素脱离文档流时不会
?	2、当容器是边框的时候也不会
?	3、当容器和子元素都是边框时也不会掉
?       解决办法：容器设置overflow:hidden;     （父元素有边框或设置overflow时，子元素浮动时，均不会掉）



盒子模型：css布局的基础，规定了网页中的元素如何显示，以及元素之间的相互关系。
?	盒模型的组成：border（边框），margin（外边距），padding（内边距），content（内容宽高）。
?	盒子内容-文本，元素
margin与padding 使用区别：
?		1）当元素内容是文本的时候，必须使用padding
?			注意点：当使用padding时，会把元素的原有大小改变，如果不想改变元素大小的原有大小，又想出现效果必须从width与height减去相应的padding值
?			当元素没有设置width和height的时候，padding值不需要减去
?		2）当元素的内容是元素时，这个时候既可以使用margin也可以使用padding，哪个简单用哪个.
?	标准盒模型宽（高）=左右margin+左右padding+左右border+width（height）
?	盒模型转换为IE（怪异）盒模型 盒子总宽度 = width（content+border+padding） + margin

	box-sizing:border-box(怪异盒模型)/content-box(标准盒模型，默认)；css3新增盒模型属性，
		诡异盒模型时，因为padding值增加的宽高不需要减去，会自动结算减掉。width包含了padding与border
	margin:0 auto;一个`有宽度`的元素在浏览器中横向居中

outline:none:去掉表单聚焦时的边框;
letter-spacing:字间距

overflow:visible/hidden()/scroll()/auto()/inherit;
?	visible:默认值，内容不会被修剪，会呈现在元素框外
?	hidden:内容会被修剪，不可见
?	scroll:不管内容是否被修剪都有滚动条
?	auto:如果内容被修剪，则浏览器会显示滚动条，否则不会显示
?	inherit:从父元素继承

pre预格式化标签 <pre></pre>块级标签，可以保留代码中的换行，空格等文本格式
code代码标签  <code></code>内联标签，专门放置源代码的标签。工作中常用pre标签代替
<u></u>下划线标签
<s></s>  <del></del>删除线标签
<sup></sup>上标
<sub></sub>下标
<nobr></nobr>强制不换行

### 空余空间

white-space:nowrap/pre/pre-wrap/pre-line;//设置如何处理元素内的空白

* nowrap:强制不换行
* pre:空白被保留，类似pre标签
* pre-wrap:保留所有空格，保留换行
* pre-line:空格只保留一个，保留换行

text-overflow:ellipsis/clip;
?	ellipsisi:当单行文本溢出时显示省略号，常与overflow:hidden一起使用;
?	clip：不显示省略号

~~~scss
// 一行超出文本显示省略号
{
    overflow:hidden;
    white-space:nowrap;
    text-overflow: ellipsis;
}
// 多行显示不下才出现省略号
{
    display:-webkit-box;
    overflow:hidden;
    text-overflow:ellipsis;
}
~~~

text-align:使文本与图片水平居中，设置在父元素上面



### 元素类型
1. 块级元素	独占一行，自上而下排列，可以设置宽和高，一般作为其他元素的容器
2. 内联元素	横向排列，不能设置宽高（标准流中不能设置宽高-脱离文档流可以设置宽高  如成为浮动流 或者定位流中的绝对流与固定流）
3. 内联块元素	横向排列，可以设置宽高

元素类型本身注意点：

1. 一般使用div去嵌套其他元素 - div网页布局，划分网页区域
2. p标签不能嵌套其他块级元素
3. 内联元素使用margin-top或者padding-top有问题，不起作用或者不规范。margin-left正常

改变元素类型：display：inline(内联)/block(块级)/inline-block(内联块)/none/list-item(转换成列表类型)
?	注意点：内联块元素不能转成内联元素,因为内联块元素默认有内在样式。但可以转换成块级元素。比如input元素
?	display:block;	1、让其他元素转换成块级元素 2、表示让一个隐藏的元素显示，并且变成块级元素
?	display:none;	让元素隐藏


内联块元素






元素垂直居中**：
?	父元素设置转成单元格类型  display:table-cell;   vertical-align:middle;   存在问题：父元素无法设置margin值，可以设置padding

水平居中：父元素text-align:center;子元素为内联元素或内联块//或者子元素设置宽，margin:0 auto;

vertical-align:middle/top/bottom;
?	1、使文字与图片垂直居中，以高度大的为参考，块级元素无效。设置在文字与图片上，并非容器上。
?	2、使元素垂直居中
?	3、解决图片间隙问题（容器没有高的情况下，图片默认将容器撑大）

置换元素：浏览器根据元素的标签和属性，
非置换元素


使用行高时注意：当行高大于容器高时



### 定位position
1. 语法  position:static(默认，静态定位)/absolute(绝对定位)/relative(相对定位)/fixed(固定定位);
2. 定位需求：需要元素层叠排列，元素固定在某个位置

#### 定位流：

1. 给元素设置了position属性，然后成为定位流
2. 定位流和标准流不是一个层次的，可以理解为在标准流之上
3. 定位属性必须配合top，left，bottom，right使用
4. 定位属性可以层叠-定位有层级关系
5. 定位属性层级关系-后面把前面的覆盖

#### 相对定位特点
?	1、相对定位不脱离文档流的，占空间，区分元素类型-就是元素时什么类型，相对定位之后还是什么类型
?	2、相对定位是相对于自己以前在标准流中的位置进行定位的
?	3、对元素进行微调，子绝父相（父元素设置相对定位，子元素绝对定位）

#### 绝对定位特点
?	1、绝对定位脱离文档流，不占空间，不区分元素类型（比如行内元素也可以设置高宽）。
?	2、相对于最近一级拥有定位流的祖先元素进行定位。如果它的上级都没有定位流，则以body定位；
?	注意：绝对定位相对于谁定位时，默认先去找上一级

#### 固定定位特点
?	1、脱离文档流，不占空间，不区分元素类型
?	2、相对于body定位
?	3、不会随着滚动条而滚动

定位实现元素垂直居中：
?	1）子绝父相，子元素top,right,left,bottom都为0，margin为auto
?	2）子绝父相，子元素left,top为50%，margin-left与margin-top为负的宽高的一半

z-index:控制定位元素的层级  auto/具体数值（没有单位，可以为负值）

透明度：标准写法opacity:数值;  取值范围0-1小数
?        IE写法 filter:alpha(opacity=value);取值范围1-100
?	隐藏元素的方法 opacity:0;

字幕滚动 <marquee></marquee>默认从右向左
?	behavior="alternate"表示左右滚动
?	direction="right"指定方向从左向右
?	scrollamount="50"指定速度

图片整合技术 (sprite 精灵图)  background-position
?	把很多张小图整合成一张大图，通过background-position属性来使用
?	可以提高加载速度，减轻服务器压力，可以减小图片的体积

序列选择器  :nth-child()
?	ul li:nth-child(2){   /*ul下的第二个li*/
?	background-position:-62px 0;
?	}

宽高自适应：height:auto;利用子元素把容器撑开
min-width：最小宽度可以伸缩到一定程度之后不再进行压缩，然后产生滚动条。同时也需要给定宽度

max-width：固定宽缩小到一定程度会产生滚动条，最大宽可以使元素到达固定值时可以自动伸缩，不产生滚动条

min-height:最小高
?	需求：当内容没有那么多的时候元素有一个固定高度，当内容多时高度可以随着内容改变

max-height:最大高  开始时自适应，超出时可加入其它动作  如超出时添加一个查看更多的按钮

关键字过滤器   !important   最高优先级 其次是内联样式

display:none;  //元素完全不存在页面中，不占位
visibility:hidden/visible;  //存在页面中，不可见而已，占位
opacity:0；

text-indent:首行缩进为负，可让文字隐藏

伪类选择器：
?	link
?	visited
?	hover
?	active
伪元素选择器
div::after{       //相当于div内多了一个元素，属于内联元素
?     content:"";/*可以为空，不能不写*/
}
div::before{
?     content:"";
}

p::first-letter{    //对p段落内的第一个文字设置字号
?      font-size:30px;
}

p::first-line{    //对p段落内的第一行设置颜色，以换行符为界
?     color:red;
}

给图片加padding，不会将图片撑大，会将图片到边框的距离撑大。


















