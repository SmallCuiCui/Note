## js week3

### tips

##### table注意点

* 在html中写的表格，渲染到浏览器会自动添加tbody和thead。使用js创建表格则不会出现
  ​	平时使用js创建表格时，注意加上thead,tbody。避免出现删除查找时的bug

##### mouseenter与mouseover差别

onmouseenter：只有在鼠标指针穿过被选元素时，才会触发 mouseenter 事件
onmouseleave
onmouseover：不论鼠标指针穿过被选元素或其子元素，都会触发 mouseover 事件

##### 添加class

* ele.className = "box ac";//此方式会完全替换元素原来的class值

* ele.classList.add('ac');//额外添加一个class值

* ele.classList.remove('ac');//删除值为ac的class

##### 循环中的变量使用

循环给一系列元素绑事件，在事件函数里不能使用循环变量  ****
​	事件的触发是在for结束之后才触发的
​	data[i].onclick = function(){
​		this.name...  //使用this,不能使用变量i来获取
}

##### 原生js读取外部json文件

~~~javascript
var url = 'studdent.json'  //要读取的json文件的路径
	var request = new XMLHttpRequest();
	request.open('get',url);   //通过get方式打开url，此时请求还未发出
	request.responseType = 'text';   //设置返回响应数据的类型
	request.send();   //发出请求，此时还未响应
	request.onload = function(){
		var text = request.response;  //得到一个对象，其类型取决于 responseType 的值
		var jsonData = JSON.parse(text);  //json方法，将字符串转换为js对象
}
~~~

### BOM浏览器对象模型

window是全局浏览器内置顶级对象，表示一个浏览器窗口。在js中，window对象是全局对象，所有的表达式都在当前环境中计算。全局变量是挂在window下的

##### window下的子对象

：document，location，navigator，history，screen，frames

###### document



###### location

~~~javascript
window.location.href;//当前页面的URL，可以获取可以修改
window.location.hostname;//web主机的域名
window.location.pathname;//当前文件的路径和文件名
window.location.port;//web主机端口
window.location.protocol;//所用的web协议（http,https)
window.location.search;//请求参数，？号后面的内容
~~~

页面刷新：window.location.reload();

###### navigator

~~~
navigator.appName;//获取当前浏览器的名称
navigator.appVersion;//获取当前浏览器的版本号
navigator.platform;//返回当前计算机的操作系统
~~~

以上属性逐渐被抛弃，取而代之的新属性navigator.userAgent  //返回浏览器的信息

###### history

~~~javascript
history.go(1);//参数可写任意整数，正数前进，负数后退
history.back();//后退
history.forward()//前进
~~~

###### screen

~~~~
window.screen.width;//返回当前屏幕宽度
window.screen.height
~~~~

##### window下弹框

* alert();//提示框，会暂停程序，点击确定后才能继续
* prompt();//弹出输入框，得到一个字符串
* confirm();//确认弹框，点击确定得到true，点击取消得到false

##### 定时器

* 超时定时器：多久之后执行，仅执行一次

  setTimeout与clearTimeout

* 间隔定时器：每隔多久执行一次

  setInterval与clearInterval

~~~javascript
var timer = setInterval(function(){
    console.log(11);
},2000);
clearInterval(timer);
~~~

采用setInterval(fun(),30)做动画，时间设置为30-60之间比较合适，动画不卡顿，也节约资源

##### window.onload

页面加载完成。js获取Dom元素必须在页面上的元素加载之后才能获取到。

##### window.onscroll

~~~javascript
//获取滚动条滚上去的距离
window.onscroll = function(){
    //滚动条兼容，逻辑短路
    var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
}
~~~

##### window.onresize

~~~javascript
//浏览器窗口大小发生变化时触发的事件
window.onresize = function(){
    console.log(1);
}
~~~

### DOM文档对象模型

##### DOM元素节点获取

​	document.getElementById(id名)
​	document.getElementsByTagName(标签名)  //类数组
​	document.getElementsByName(name值)   //类数组  低版本浏览器不兼容
​	document.getElementsByClassName(class值)  //类数组  IE8下不兼容

ES5选择器：也是元素的方法，相当于给选择加了限制条件
​	document.querySelector('value');  //value可以是类名，id名，标签名。一旦匹配上就不往后匹配了
​	document.querySlectorAll('value');  //value可以是类名，id名，标签名。类数组，得到所有匹配，支持IE8+
​	element.children属性：获得所有孩子元素，类数组
​	element.parentNode属性：获得该元素的父元素	

##### 操作DOM：增，删，克隆

* 创建节点:
  ​		var odiv = document.creatElement('div');

* 插入节点：
  ​		parentNode.appendChild(newNode);  //将新节点追加到子节点列表末尾
  ​		parentNode.insertBefore(newNode,targetNode)  //将新节点插到父节点下目标节点的前面

* 克隆节点：
  ​		cloneNode = Node.cloneNode(boolean)  //参数为true表示复制该节点以及其下的所有子节点

  ​		false表示只克隆该节点

* 替换节点：
  ​		parentNode.replaceChild(newNode,targetNode)  //使用新节点替换目标节点

* 移除节点：
  ​		parentNode.removeChild(childNode);  //移除目标节点
  ​		node.parentNode.removeChild(node);  //不清楚父节点的情况下使用

##### 获取属性值

：元素的方法，不能使用document调用。

* element.attributes  //获取所有该节点的信息，只做获取。是个属性，不是方法，

* element.getAttribute()  //自定义属性必须用getAttribute()和setAttribute()

* element.setAttribute(,)  //添加指定的属性并为其赋指定的值(值为字符串类型)，审查元素时会显示在DOM元素上。IE8以下存在兼容问

* element.removeAttribute()  //删除属性

* 采用点(.)获取  //只能获取该元素原型上的属性(如id,className,style)，以及使用(.)添加的属性(此方式设置的属性不会显示在dom上)。

  通过sel.style.xx方式设置的样式会显示在DOM上，可以通过sele.style.xx获取

##### 获取样式

ele.style.attr  //通常用于赋值。只能获取内联样式的样式，通过css设置的样式无法获取

ele.currentStyle[attr]   //IE获取样式

getComputedStyle(ele,false)[attr]  //false的意思是可以获取伪元素

~~~javascript
//样式获取存在兼容性
 function getStyle(obj, attr) {
	if(obj.currentStyle){
		// IE
 		return obj.currentStyle[attr];
	}else{
		return getComputedStyle(obj, false)[attr];
	}
}
~~~

##### 节点(所有)

：html中的所有内容都是节点，包括标签，文本，注释等

节点类型包括，元素节点，文本节点，属性节点

* 节点操作：元素的方法
  ​	childNodes  //获取元素的所有节点，是一个数组，里面包括文本节点，元素节点，注释节点等所有节点
  ​	nodeValue  //获取*文本节点*的文字内容，并且可以赋值，会替换原内容。可获取属性节点的值
  ​	box.childNodes[0].nodevalue = '<strong>abc</strong>' ;//文本节点的属性，其他类型节点使用没效果。不会被解析成标签，会将标签名转义为字符串直接输出
  ​				box.innerText=  '<strong>abc</strong>' ;不会解析标签，替换box节点下的所有节点
  ​				box.innerHTML=  '<strong>abc</strong>' ;//标签会被解析，替换box节点下的所有节点（非w3c）
  ​				box.outerHTML=  '<strong>abc</strong>' 会解析，但包含自己一起被替换（非w3c）
  ​	nodeType   //节点类型，返回值为数字
  ​			文本节点,3，元素节点：1，注释：8，属性节点：2，document节点：9
  ​	nodeName  //返回节点名称（#text(文本节点)，#comment(注释)，标签名，属性名(document)）

##### 获取相关元素

###### 获取孩子

ele.childNodes;//得到子节点的集合，包括空格与换行(可以通过判断得到所有子孙元素)

ele.children;//得到子元素的数组，只能得到一级子元素

ele.firstChild;//得到第一个子节点，包括空格与换行

ele.firstElementChild;//得到第一个子元素

ele.lastChild

ele.lastElementChild

###### 获取父元素

两种方法的结果通常一样，文本节点不可能作为父节点，它本身就是一个节点

ele.parentNode;//父节点，可以找到document，

ele.parentElement;//父元素，找到document的时候会报错

###### 获取兄弟

通过父元素获取ele.parentElement.children[1];

ele.previousElementSibling;

ele.previousSibling;   //包含换行和空格

ele.nextElementSibling;

ele.nextSibling;   //包含换行和空格


##### 文档碎片

js创建元素之后，先放到文档碎片中存储起来，等所有元素都创建完之后在一次性使用appedChild加入DOM树中去，可以提高效率

~~~javascript
//创建文档碎片，在内存中
var chach = document.sreateDocumentFragment();  
for(){
    //创建li
    chach.appendChild(li);  //内存中操作，速度快，减少DOM操作次数
}
ul.appendChild(cache);  //只有一次DOM操作
	
~~~

##### DOM尺寸

```javascript
//以下属性只可读，不可更改
box.offsetWidth  //div的宽高，包含border和padding
box.offsetheight
box.clientWidth  //div宽高，包含padding，不包含border
box.clientHeight
box.scrollWidth  //可滚动内容的宽高
box.scrollheight
```

##### DOM坐标

* box.offsetLeft  //相对于元素当前相对于offsetParent的位置。

* box.offsetTop

  offsetParent：定位父级

  * 元素定位fiexd定位，定位父级为null
  * 元素非fiexd定位，父级均无定位，则offsetParent为body
  * 元素非fiexd定位，父级存在定位，则offsetParent为最近的定位父级

##### 获取body尺寸

~~~
width = document.documentElement.clientWidth || document.body.clientWidth,
height = document.documentElement.clientHeight || document.body.clientHeight
~~~



### 事件

事件是给浏览器定义一个预处理函数，当事件触发的时候执行函数

##### 兼容问题

~~~javascript
//事件对象兼容
e = e || window.event;
//阻止默认行为
//oncontextmenu鼠标右键
//onsubmit表单提交事件 
document.oncontextmenu = function(e){
    if(e.preventDefault){
		e.preventDefault();
	}else{
		e.returnValue = false;  
	}
}

//阻止冒泡
if(e.stopPropagation){
    e.stopPropagation();
}else{
    e.cancelBubble = true;
}
//事件委托
//监听事件的绑定与取消绑定
//鼠标滚轮事件
~~~

##### 事件句柄

：click,mousedown,mousemove,mouseup,keydown,keyup,keypress不包含on

元素节点.on+事件句柄 = function(e){

​	e = e.window.event;

​	处理事件....

}

##### 鼠标事件

| 属性名                | 含义                                                 |
| --------------------- | ---------------------------------------------------- |
| e.buttons             | 返回鼠标点击按键（1左键，2右键，4中键滚轮）          |
| e.clientX\|e.clientY  | 获取鼠标距可视区的距离（根据浏览器定位）             |
| e.offsetX\|e.offsetY  | 获取鼠标距事件源左上角的坐标                         |
| e.screenX\|e.screentY | 获取鼠标距屏幕左上角的坐标，包括浏览器上面的导航栏区 |
| e.pageX\|e.pageY      | 获取文档坐标，相对于整个document定位(包含滚动条)     |

##### 键盘事件*

(document的方法)键盘上每一个键都有唯一的编码

存在兼容问题：var code = e.keyCode ||e.which

特殊键码(boolean类型) e.altKey|e.ctrlKey|e.shiftKey

~~~javascript
//判断组合键
document.onkeydown=function(e){
    //解决兼容
	e = e||window.event;
    //解决兼容
    var code = e.keyCode ||e.which;
    if(code ===13&&e.altKey){
        alert('同时按下enter和alt');
    }
}
~~~

##### 鼠标滚轮*

~~~javascript
//给元素绑定鼠标滚轮事件
var scroll = function (obj, fn) {
	// fn为回调函数，函数作为参数传递
	// 判断事件有没有效，而不是有没有绑定（有效但是没有绑定的时候值为null）
	if(obj.onmousewheel !== undefined) {
		obj.onmousewheel = function (e) {
			e = e || event;
			fn(e.wheelDelta < 0);
		}
	}else{
		obj.addEventListener("DOMMouseScroll", function (e) {
			e = e || event;
			fn(e.detail>0);
		})
	}
}
~~~

~~~javascript
//获取滚轮顶部的距离scrollTop
//兼容各浏览器
var scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop;
~~~

##### 事件流

​	:事件执行顺序，捕获阶段->目标阶段->冒泡阶段
​	事件处理是在冒泡阶段进行。子元素事件触发之后，父元素的相同事件也会被随之触发
​	采用事件监听可以设置事件处理在捕获阶段进行。
​	IE浏览器不认识捕获，但是有捕获的

##### 事件绑定

：DOM0级事件处理，同一个元素的同一个事件只能绑定一个处理函数，如果绑定多个，后面的会覆盖前面的。

~~~JavaScript
div.onclick = function(){}
~~~

##### 事件监听 *

：DOM2级事件处理，可以重复监听，同一事件可有多个处理函数。存在兼容问题

~~~javascript
//事件监听绑定兼容
if(window.attachEvent){
    div.attachEvent("onclick",function(){});//IE下，只有冒泡阶段，所以没有第三个参数，注意有on
}else{
    div.addEventListenner("click",function(){},true); 
	//参数一：事件句柄，，没有on
	//参数二：处理函数
	//参数三：是否捕获,true:表示在 捕获阶段进行处理函数，即先触发父元素的处理函数，然后才是子目		
}

//移除事件监听
if(window.detachEvent){
    div.detachEvent("onclick",fun);
}else{
    div.removeEventListener("click",fun,false);
}
~~~

##### 事件委托*

：利用*事件冒泡*，只指定一次事件处理程序，处理某一类型的所有事件

优点：

* 只需要绑定一次事件，把事件委托给父级。效率更高
* 事件源不确定下没法绑定事件，通过事件委托为新加入元素绑定事件

事件源：获得点击的具体元素

~~~javascript
var target = e.target || e.srcElement;//存在兼容问题
var tar = target.nodeHTML;//target是一个事件对象的一个对象，获得标签名来判断事件源
~~~



​	

