
分辨率：指宽度上和高度上最多能显示的物理像素点个数
点距：像素与像素之间的距离。点距与屏幕大小决定了分辨率大小
DPI每英寸像素密度
PPI屏幕像素密度

XHTM与HTML区别：
	XHTM元素必须正确嵌套，元素必须闭合，标签必须小写，必须有根元素

text-transform：capitaize 每个单词首字母大写
font-variant:small-caps；把文本设置为小型大写字母

单位 rem，可根据根元素html的font-size大小来计算，根字体大小为15px，则1rem=5px

粘性定位：position：sticky；必须给定top值定位
	

Doctype作用：定义文档类型，让浏览器解析器知道应该用哪个规范来解析文档。声明在html第一行，不是html标签
	严格模式：浏览器按照w3c标准解析代码。文档包含doctype即为严格模式
	混杂模式：怪异模式或兼容模式。指浏览器按照自己的方法解析代码

网页优化：
	1、H1标签的使用，它的搜索权重比其他标签高，多用于包裹logo
	2、页面头部优化
	   <meta name="keywords" content=""/>网站关键字，不要设置过多，搜索引擎一般只会浏览靠前几个关键字
	    <meta name="description" content=""/>描述网站,
	3、超链接优化，为爬虫创造良好的爬行通道
	     为a设置title属性，既可以起到提示访客的作用，也可以让搜索引擎知道它要去哪里
	     最好不要使用图片热点链接
	4、图片优化
	     使用alt属性与title属性
	

媒体查询
 div{
	height:200px;
	backgound-color:red;
}
/*设置屏幕宽度在800-1000之间的样式*/     all // only screen
@media all and (min-width:800px) and (max-width:1000px){
      div{
	height:200px;
	backgound-color:red;
      }
}

/*竖屏*/
@media only screen (orientation:portrait){
      指定样式
}
/*横屏*/
@media only screen (orientation:portrait){
      指定样式
}


过渡  transition:all 2s 2s;
	第一个值：transition-property:需要过渡的属性。多个属性之间逗号隔开
	第二个值：transition-duration过渡时间
	第三个值：transition-delay延迟时间
	transition-timing-function过渡类型 linear(),ease(),ease
	过渡对display:none;不起作用

位移 transform：50px 100px;
	位移 transform: translate(x,y)在水平/垂直方向上位移
		translateX()
		translateY()
		translateZ()沿z轴进行位移
	        transform: translate3d(x,y,z) 3d位移

	旋转  transform: rotate（45deg,45deg）
		rotateX(45deg)水平方向
		rotateY(45deg)垂直水平方向
		rotateZ(45deg)z轴方向
	         transform:rotate3d(value,value,value,deg);value取值为0或1,1表示旋转，0表示不旋转。第四个值是旋转角度值

	缩放  transform: scale(1,2);值大于一放大，小于一缩小
		scaleX(1)水平方向
		scaleY(2)垂直方向
		scaleZ();
		scale(1.5)只有一个值的时候表示两个方向一起缩放

	transform-origin(value1,value2)设置运动参照点,也可以取值top,right,left,nottom。默认点在元素中心

	空间类型  transform-style:flat(默认,2d)/preserve-3d(3d);

	perspective:500px;透视(景深)属性，方便观察旋转效果。最好写在父元素上面。子元素写法transform:perspective(500px);
	
	两个transform同时存在时，后一个会覆盖前一个

动画animation 动画三要素
	1、动画名animation-name
	2、动画执行体 @keyframes myname{    //myname是定义的动画名称
				from{ }  //或者写百分数0%~100%
				to{ }
				}
	3、动画持续时间animation-duration:3s;

        动画过渡类型animation-timing-function:
	linear匀速
	ease
	ease-in由慢到快
	ease-out由快到慢
	ease-in-out慢到快到慢
	**step-start马上跳到动画每一结束帧的状态

          动画延迟时间animation-delay：2s;

          动画循环次数animation-iteration-count:infinite(无限次)/number;

          动画循环是否反向animation-direction
	nomal:正常方向
	reverse:反方向
	alternate:先正后反，并持续交替进行
	alternate-reverse:先反后正并持续交替进行

           设置对象动画状态animation-play-state:running(运动)/paused(暂停)

     复合属性 animation:mtmove 2s ease-in-out 2s infinite alternate;
	动画名，时间，过渡类型，延迟时间，循环次数，动画方向


animation动画与transition过渡的区别
	相同点：都是随着时间改变元素的属性值
	不同点：transition需要触发事件(hover//click等)才能随时间改变css的属性值
	 	animation无需触发也可以随时间显示的改变css属性值


线性渐变  从一个方向到另一个方向
	 background:linear-gradient(to left,red,yellow)  从右到左由红变黄
	角对角 background:linear-gradient(to left bottom,red,yellow)从右下角到左上角。指定两个值，先左右，后上下
	角度 background:linear-gradient(45deg,red,yellow) 0deg时是从下往上 

径向渐变 以中心到四周渐变，具有兼容问题
	background:radial-gradient(ellipse,red,blue,yellow)


元素垂直居中，变形属性里面的位移配合定位实现元素居中

一个元素如何在页面中显示，由元素类型和盒模型决定
	盒模型：css布局的对象和基本单位
	元素类型：即display属性，不同属性值决定不同盒类型，从而决定元素不同渲染方式

BFC块级格式上下文：决定元素如何对其内容进行定位，以及与其他元素的关系和相互作用
	BFC触发：1、浮动元素，float除了none以外的值
		2、position的值为absolute或fixed
		3、display的值为inline-block，table-cell，table-caption
		4、overflow除了visible以外的值
	BFC应用：1、解决浮动塌陷问题
		2、自适应两栏布局（运用bfc阻止元素被浮动元素覆盖的特性来实现自适应两栏布局）
		      方法：第一栏采用左浮动，第二栏设置overflow:hidden;
		3、解决margin值重叠问题




