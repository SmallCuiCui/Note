采用百分比的高度，设置行高等于容器高来居中文本不起作用；

height:calc(234px - 14px);//加减乘除都可以

ul>(li>a{子选择})*3 

选择器 
      ul li{}  空格表示后代选择器，选中儿子孙子
      ul>li{}  子选择器，只选中它的子元素，孙子元素不会选中
      交集选择器，div.box{}选中class为box的div，区别于中间有空格的后代选择器。
      序列选择器nth-child不建议外层使用，不能被其他元素隔开


----**兼容性问题**----

浏览器
        3、浏览器内核：渲染引擎
	Trident（MSHTML） 代表作：IE，腾讯，theworld世界之窗，360浏览器
	Gecko(壁虎)   Mozilla Firefox，跨平台
	Presto(迅速的)   Opera前版本
	Webkit(Safari内核，Chrome内核原型，它是苹果公司自己的内核，也是苹果的Safari浏览器使用的内核)
	Blink（Google和Opera Software）

为什么会出现浏览器兼容问题：由于各大主浏览器由不同厂家开发，所用的核心架构和代码很难重合
css bug:css样式在各浏览器中解析不一致的情况
css hack:css中指一种兼容css在不同浏览器中正确显示的技巧方法，是属于个人，非官方的补丁
Filter：一种对特定浏览器或浏览器组显示或隐藏规则或声明的方法，过滤不同浏览器的hack类型

hack解决浏览器兼容问题的方法

浏览器兼容问题：

	图片问题
	1、div中插入图片，默认将div撑大两个像素  hack：将<img>转为块级元素  display:block;
	2、图片横着并排列，图片之间存在间隙  hack：浮动img{float:left}
	3、dt与li中图片间隙问题  hack：将<img>转为块级元素  display:block;
	4、图片将容器撑大，hack：图片设置vertical-align:top;  

	图片在IE低版本会出现蓝色边框   hack： img{border:0;}

	表单在各浏览器的表现不同，距离顶部的距离不一致。采用浮动，去掉默认边框。

	按钮标签默认样式不一致。去掉边框，给定高宽，更换背景//或用a标签模拟按钮。//采用input取代，去掉input边框，在外面套一个标签写样式

	IE上不能使用序列选择器

	IE百分比问题：百分之五十加百分之五十大于百分百，hack： clear:right;

	鼠标指针bug，IE低版本中鼠标变小手可以使用cursor:hand;  hank: cursor：pointer；

	IE低版本中，input类型为text时内容不居中，hack：设置line-height=height
	cursor：move;
	
	当li中的a转换成block类型，并且有height与float时，li中没设置浮动会出现阶梯显示。hack：同时给li添加float

	下划线过滤器_height
	需求：希望最小高度在标准浏览器与低版本的IE中也能正常使用。
	1、标准浏览器下正常高度和最小高度同时出现，最小高度不起作用
	2、正常高度在低版本IE下就是指最小高度
	hack：让他们同时存在。添加_height值，IE识别，标准浏览器不识别



----**表单**----

表单<form>  action属性：填写地址，提交之后
	target:_blank新窗口打开
	name
	method:get(不安全)/post ***
	novalidate:novalidate;对表单进行不验证提交。取消默认验证，如email表单的默认验证

h5中，如果属性和属性值一样的话，可以省略属性值。

<input type="email"/>输入邮箱，有默认的邮箱格式验证，h5新增
	工作中使用js正则表达式进行验证

method:post/get区别
	1、get是从服务器获取数据，post是向服务器传送数据
	2、get是把参数数据队列添加到提交表单的action属性所指的URL中，在URL中可以看到，故而不安全。而post是通过HTTP post机制，用户看不到
	3、get传送数据量小，速度快。post传送数据量大，速度慢于get

表单分组<fieldset disabled="disabled"></fieldset>相当于一个方框，将表单分组。fieldset可嵌套。内部设置disabled可定义整个组禁用
	 配合<legend>给分组命名  <legend>在fieldset对象绘制的方框插一个标题，在一个fieldset内唯一
	legent问题，text-algin:center不仅会使文字居中，还会使legent居中，故不设置宽，常用padding撑大，使文字居中.使用margin-left定位

<lable></lable>将信息绑定到表单控件上，表单的ID与lable的for值相同
	lable第一种写法：
	           <lable for="sl">同户名</lable>  <input type="text" id="sl"/>  获取焦点(点击用户名，同名ID的input可获取焦点)需要绑定，ID名称不能一样，具有唯一性
	lable第二种写法：
	           <lable for="sl">同户名 <input type="text" id="sl"/> </lable> 不需要绑定

上传文件框：
<input type="file" multiple="multiple"/>  可选取文件上传，样式较丑，使用opacity:0;隐藏，使用容器给样式  multiple表示可选取多个文件

图像域：
<input type="image" src="lujing"  width="" height="" alt=""/>使用图片作为按钮。火狐低版本不支持




----**表格**----

表格<table>
	表格行分组： thead(表头),tbody(表体),tfoot(表尾)
	
	表格列分组：<colgrounp span="1"></colgroup>列分组，选中第一列，再单独给样式

	<caption>表格标题，必须写在table标签内部
`		caption-side:top/right/bottom/left

	border-spacing:value; 单元格间距   表示单元格边框之间的距离，必须给table添加，不可取负值

	border-collapse  合并相邻单元格边框 border-collapse:separate(边框分开，默认)/collapase(边框合并)

	empty无内容时单元格的设置empty-cells:show/hide;

	隔行变色
		双数：tr:nth-child(2n){} (even)
		奇数：tr:nth-child(2n+1){} odd

	html重要属性：
		colspan="value"：合并列
		rowspan="value"：合并行
		valign="top/bottom/middle/baseline"垂直对齐方式
		rules="rows/cols/all/none"添加组分隔线  会去掉单元格的边框，然后再以行或列添加分隔线
	

标题栏图标 <link rel="shortcut icon" href="图标路径"/>


h5基础
1、新增了一些标签和属性。增加了语义化的标签，header/nav/main/footer...增加了视频，音频标签

h5文档定义就一种<!doctype html>
h4的文档定义类型：strict（严格型）、transitional(过渡型  常用)、frameset(框架型)、手机浏览器DTD mobile

DTD：规定标记语言的规则，这样浏览器才能正确的呈现内容

SGML标准通用标记语言,是一种定义电子文档结构和描述其内容的国际标准语言

语义化的重要性
	1、当页面加载失败时，还能呈现清晰的结构
	2、有利于SEO优化，利于被搜索引擎收录，即利于网络爬虫的识别
	3、在项目开发及维护时，语义化可以很大程度上降低开发难度，节省成本。

在IE低版本浏览器下，当不定义doctype会触发怪异盒子



------**多媒体标签**-----

<video width="500" height="300" controls="controls">
	<source src="video/movie.mp4" type="video/mp4"></source>
	<source src="video/movie.ogv" type="video/ogg"></source>
	<source src="video/movie.webm" type="video/webm"></source>
	<!--给低版本浏览器做提示作用-->
	当前版本不支持video直接播放，点击下载视频:<a href="myvideo.webm">下载视频</a>
</video>
定义音频
	<audio src="" loop="loop"></audio>
定义视频
	<video src="视频路径" controls="controls" loop="loop"></video>

	src:路径
	controls:视频控件属性，必给这个属性，不然无法播放
	loop:循环播放
	autoplay:自动播放
	muted:静音
	poster:视频封面，放封面图片路径

在谷歌下视频进度条不支持，autoplay被禁用

视频标签只支持 Webm,ogg,mp4(MPEG4)三种视频类型



----**新增语义化标签**----

<section>划分文档的某个区域

<article></article>定义独立内容

<aside></aside>定义所处内容之外的内容。如侧边栏，广告

<header></header>网页头部
<nav></nav>导航栏
<main></main>文档主体，网页中只能有一个
<footer></footer>底部

<mark></mark>高亮显示文字

<address></address>地址标签，自带斜体效果

<figure></figure>用作文档中插图的图像，表示文档主体内容中一个独立的单元
	使用<figcaption>元素为figure元素添加标题，从属于figure，且唯一。类似自定义dl

<canvas>图形画布


----**新增表单元素**----

新增type属性值
	email：用于专门输入email地址的文本框，如果该内容不是email地址格式将不允许提交。
		具有multiple属性值，它允许该文本框中输入一串以逗号分隔的email地址
	
	url：专门用于输入URL地址的文本框，如果内容不是地址，则不允许提交。和require属性配合使用

	number：专门用于输入数字的文本框，输入时检查内容是否为数字。具有min，max，step的属性
		min，最小值
		max，最大值
		step 数字间隔

	range:只允许输入一段范围内数值的文本框，它具有min，max，step的属性

	data pickers:可供选取日期和时间，month，week，time，datatime，datatime-local


<datalist>提供一个事先定义好的列表，通过ID与input关联，当在input内输入时，用户会看到一个下拉列表供其选择
	<input list="words"/>
	<datalist id="words">
	    <option value="firefox"></option>
	    <option value="firefox"></option>
	</datalist>

新增表单验证：
multiple:可输入多个
novalidate：属性规定当提交表单时不对其进行验证，可取消表单元素的默认验证。给form添加
required:验证内容是否为空，表单为空不能提交
autofocus：给文本框，选择框或者按钮控件加该属性时，当打开网页时该表单控件自动获得焦点，一个页面只能有一个
min 最小值是多少
max  最大值是多少
step 间隔值是多少
maxlength="value" 表单允许输入最大字符数，超过这个数值将无法输入
autocomplete:on(记录)/off;   记忆属性，会记住表单的输入记录，设置在form上。表单需要给定name，区分记录。


-----**CSS3**-----

css层叠：指样式表的优先级，当发生冲突时以优先级高的为标准

c3新特性：选择器，图片视觉效果（圆角，阴影，渐变背景，图片边框），背景应用，盒模型的变化，阴影效果（文本阴影，盒子阴影）
	多列布局，弹性盒布局，web文字和font图标，颜色和透明度，圆角和边框新特效，2D和3D变形，css3过渡和动画效果，媒体查询和Responsive布局

css选择器
1、属性选择器
	E[strr]只使用属性名，没有任何属性值
	E[strr="value"]指定属性名，且给定完整属性值或属性值组。该属性的属性值必须写全，如果有多个，也需要全部写上才能选中
	E[str~="value"]指定属性名，给定属性值组中的一个属性值。属性值组中可能有多个值，只需写一个即可选中
	E[str^="value"]指定属性名，给定整体属性值的开头部分。若属性值中有多个，只能匹配第一个的开头
	E[str$="value"]指定属性名，给定整体属性名的结尾部分。若属性值中有多个，只能匹配最后一个的结尾。
	E[str*="value"]给定属性名，给定属性值中的一份即可。

2、伪类选择器

   UI元素状态伪类
	p::selection{  }可以让鼠标选中文字的时候给其设置文字颜色和背景。不能设置其他属性。注意是两个，冒号
	E:checked{}匹配所选范围内所有处于不可用状态的E元素
	input:disabled{}匹配不可用状态（禁用）的元素
	input:enabled{}匹配可用状态的元素

    序列选择器
       1)同级别的第几个
	E:first-child  选中同级别的第一个标签。	要求 同级别，同类型，不能被隔开  被隔开的话会出现混乱
	E:last:child 选中同级别的最后一个标签 	 要求 同级别，同类型，不能被隔开
	E:nth-child(n)选中同级别的第n个标签  	 要求 同级别，同类型，不能被隔开
	E:nth-last-child(n)选中同级别的倒数第几个标签	要求 同级别，同类型，不能被隔开
	E:only-child 选中父元素中唯一的标签。该选中标签E是其父元素唯一的一个孩子
      2)同类型*的第几个
	:first-of-type{} 选中同级别同类型的第一个
	:last-of-type{} 
	:nth-of-type(n){} 
	:nth-last-of-type(n){} 
	:only-of-type{} 

   否定选择器:not() 定位不匹配该选择器的元素
	例如选中input中除了type类型为tel的表单 input:not([type="tel"]){border:1px solid red;}

   根选择器 :root{background-color:red;}  直接写，前面不需要加元素，是将整个屏幕背景设置为红色
	html与body的高度是有子元素撑开的，但是给body背景颜色时是给整个屏幕

3、层次选择器 E,F都是元素
	E F后代选择器，包括子元素与孙元素
	E>F子元素选择器，不包括孙元素
	E+F相邻兄弟选择器。不能被隔开
	E~F通用兄弟选择，




浏览器的私有前缀：当一个属性没有成为w3标准时，浏览器为了支持这个属性就需要加上对于的浏览器前缀
	-moz-  火狐
	-o-  欧朋
	-webkit-  谷歌
	-ms-  IE

圆角属性border-radius
	当圆角属性值等于或大于自身元素的宽高时，就是一个圆
	border-top-left    border-top-right
	border-bottom-right     border-bottom-left

英文字母换行：word-break:break-all(换行)/keep-all(不会换行，默认)
	英文单词可以换行，连续字母和数字不会自动换行

透明   1、opacity:.5;
          2、background：rgb(0,0,0,.5);第四个值就是透明度
         区别：opacity的透明度会被子元素继承，且子孩子不能取消，而rgb的透明度不会被继承

文字阴影 text-shadow:5px 5px 5px red;  水平，垂直，模糊程度，颜色
盒子阴影 border-shadow5px 5px 5px red;


背景
     多重背景：background:url(img.png) no-repeat,url(img.png);可插多张背景图，前面覆盖后面

    背景大小   1、background-size:200px 200px;不等比拉伸
	2、background-size:100% 100% ;不等比拉伸，直到充满容器
	3、background-size: auto 100px;定背景图高，宽等比拉伸
	4、background-size:200px auto ;固定宽，高等比拉伸
	5、background-size:cover ;背景等比放大，直到容器宽高都充满
	6、background-size: contain;背景图片等比缩放，直到宽或高占满容器
	7、默认：按照背景图片大小，过大的话部分显示不下

     背景图像位置
	background-origin:pading-box(默认)/border-box/content-box
	padding-box:从padding区域开始显示
	border-box:从边框开始显示，相对于边框定位
	content-box:背景图相对于内容来定位

      background-clip背景剪裁属性是从指定位置开始绘制。
	background-clip属性 padding-box border-box content-box




-----**弹性盒布局**-----

弹性盒布局 - 伸缩盒模型布局 - flexbox
	一种新的布局形式  float position

定义弹性盒：display:flex;表示让容器变成弹性盒布局模式
	弹性盒布局模式   作用：让容器有能力控制子元素的位置，大小，顺序等
	弹性布局默认让元素横向排列
	弹性盒没法使行高压缩

flex-direction  伸缩流方向  主要用来创建主轴，定义伸缩项目在伸缩容器中的方向  主轴方向变为垂直方向后，宽高依旧是原来的
	方向改变后，相应的主轴与侧轴的方法方向也随之改变	
	:row；默认值，从左向右排列
	flex-direction:roe-reverse;
	flex-direction:column;自上而下
	flex-direction:column-reverse；

 justify-content  主轴对齐
	flex-start:伸缩项目沿一行的起始位置靠齐  默认
	flex-end:伸缩项目向一行的结束位置靠齐
	center:伸缩项目向一行的中间位置靠齐，中间没有间隙，整体中间靠齐
	space-between:伸缩项目平均的分布在一行里，两边没有留白
	space-around:伸缩项目平均分布在一行里，两边有留白
	space-evenly：伸缩项目平均分布在一行里，两边有留白。但是留白与子项目之间的距离是一相同的。

align-items  侧轴对齐 伸缩项目在侧轴上的对齐方式
	flex-start:
	flex-end:
	center:
	baseline:伸缩项目会根据伸缩项目基线对齐
	stretch:伸缩项目会拉伸填充整个伸缩容器。若伸缩项目给定高度了则不会，高度可给auto
		当容器设置弹性盒后，如果子项目没有给定高，也会默认拉伸高充满整个伸缩容器


flex-wrap:换行，把容器变成弹性盒后子元素会压缩，默认不会换行，全部排在一行
	wrap:换行
	no-wrap：不换行，默认
	wrap-reverse,反向换行，区分主轴水平还是垂直的情况。

flex-flow：弹性盒子伸缩方向 与 换行的缩写
	flex-flow: wrap;
	flex-flow: column wrap;
	两个值单独定义或只给一个值都可以，不区分先后顺序

align-self:(加在子元素上)单独定义一个子元素在侧轴的对齐方式
	flex-start,flex-end,center,stretch

align-content 堆栈伸缩行（行与行之间的对齐方式）
	flex-start:各行沿起始位置靠齐  默认
	flex-end:伸缩项目向一行的结束位置靠齐
	center:伸缩项目向一行的中间位置靠齐，中间没有间隙，整体中间靠齐
	space-between:伸缩项目平均的分布在一行里，两边没有留白
	space-around:伸缩项目平均分布在一行里，两边有留白

order:显示顺序，加在子元素上，值越小越先显示。默认按照标准流显示

flex:设置在子元素上面，可以让子元素延伸占满整个伸缩盒的空余空间。多个子元素一起设置可进行等比划分，不管子元素的宽度，将所有空间等比分。值越大分的越多

flex-grow:属性定义子项目的放大比例，默认为0，即如果存在剩余空间，在子元素宽度的基础上，将剩余空间按比例分配。值越大分的越多。区别于flex

flex-shrink：定义子项目的缩小比例，默认为1.当盒子变为弹性盒后，子元素压缩，因为都有这个为1的默认值。设置为0后不压缩。

flex-basis:value；类似于宽，但伸缩盒中子项目被压缩的话设置宽不起作用。就用这个设置。它是在原有宽度上增加这个value

flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto

