### DOM处理

##### 内部插入

* .append()在所有匹配元素内部末尾插入，作为子元素

* .prepend()在所有匹配元素内部开头插入，作为子元素

  ~~~javascript
  $("#div").append("<p>插入元素</p>");//在id为div的元素内部末尾插入一个p标签
  ~~~

* .appendTo()  同上

* .prpendTo() 同上

  ~~~javascript
  $("<p>插入元素</p>").appendTo("#div");//在id为div的元素内部末尾插入一个p标签
  ~~~

##### 外部插入

* after()  在所有匹配元素的后面加入内容，作为兄弟元素

* before()  在所有匹配元素的前面加入内容，作为兄弟元素

  ~~~javascript
  $("#div").affter("<p>插入元素</p>");//在id为div的元素后面插入一个p标签
  ~~~

* insertAfter()

* insertBefore()

  ~~~javascript
  $("#div").insertAfter("<p>插入元素</p>");//在id为div的元素后面插入一个p标签
  ~~~

##### 删除

* empty()  移除匹配元素的所有子元素(包括子元素，后代元素，文本等全部)

  ~~~javascript
  $("#hellow").empty();//删除id为hellow内的所有内容  无参数
  ~~~

* remove()  移除匹配元素自身及其其孩子（同时删除相关数据与绑定事件）

  ~~~javascript
  $("p").remove();//从DOM中移除所有p标签
  $("p").remove(".hellow");//从DOM中移除带有hellow的p标签,参数做过滤作用
  ~~~

* detach() 同remove，但不删除数据与事件，应用于移走后又需要插入的元素

  ~~~javascript
  $("p").detach(".hellow");//从DOM中移除带有hellow的p标签
  ~~~

### 属性

##### 属性

* attr() 获取第一个匹配元素的属性值  修改/添加所有匹配元素的属性  自定义属性

* removeAttr()  移除所有匹配元素的属性（删自定义属性）

  ~~~javascript
  $(".ele").attr("name");//获取第一个匹配的元素的name属性值
  //如果要获取所有匹配元素的属性值需要使用.each()或.map()方法
  $(".ele").attr("name","user");//设置所有匹配元素的name值为user
  //一次设置多个
  $("#div").attr({
      name:"user",
      title:"爱学习"
  })
  //通过函数返回属性值
  $("#div").attr("name",function(){
  	return "user"
  })
  ~~~

  ~~~javascript
  $("img").removeAttr("src");//移除所有img元素的src属性
  ~~~

* prop() 同attr  处理固有属性  如复选框的checked使用prop()，attr()方法获取为undefined

  * 获取元素本身及其孩子元素的html `$(".test").prop("outerHTML");`

* removerProp()  注意不要用此方法删除原生属性

##### css

* addClass(className)  为所有匹配元素添加类名

* removeClass()   移除所有匹配元素的一个、多个或全部类名

* toggleClass()  有就删除，没有就添加

  ~~~javascript
  $("div").addClass("myclass");
  //通过函数返回
  $("div").addClass(function(index){
      return "myclass"+index;
  });
  $("div").removeClass();//移除所有div的所有样式
  
  $("#ele").toggleClass("show");//匹配元素存在show样式就删除，不存在就添加
  ~~~

##### html代码/文本/值

不传参就是获取第一个匹配元素的对于内容，传参就是设置所有匹配元素的对于内容

* html()  获取第一个匹配元素内的所有内容   传参时将替换选中元素内的所有内容，可以解析标签
* text()  获取第一个匹配元素内的所有 文本节点  传参时不解析标签
* val()  常用于获取表单元素的value值（input,select,textares）

### 样式

##### css

* css(  propertyName ) 获取第一个匹配的元素的指定样式

* css(  propertyName ,value) 设置所有匹配元素的制定样式值

  ~~~javascript
  $("div").css(propertyName,value)
  $("div").css('width',function(){return 100})
  $("div").css({
      "color":"red",
      "height":"100px"
  })
  ~~~

##### 位置

* offset() 相对于文档的位置，返回包含top与left值  设置坐标  （适用于拖拽方式）

  ~~~javascript
  //获取
  $("#ele").offset().top
  $("#ele").offset().left
  //设置
  $("#div").offset({
      left:10,
      top:20
  })
  ~~~

* position()  相对于最近被定位过的祖先元素

* scrollTop()  获取第一个匹配元素的当前垂直滚动条的位置或设置每个匹配元素的垂直滚动条位置。

* scrollLeft()

##### 尺寸

* height()  获取高  返回不带单位的高度数值   区别于.css('height')返回带单位的高
* width()
* innerHeight()  包括padding值
* innerWidth()
* outerHeight()  包括padding与border  margin可选  参数为true(false默认)表示包含margin
* outerWidth()

### 选择器

##### 基本

* #id  根据id选择，得到单个元素
* .class  选定给定样式的所有元素
* element   给定标签名选择所有元素  
* selector1,selector2,selector3  传多个选择器，使用逗号隔开   返回每个选择器匹配的元素
* 组合选择器(选择器之间紧挨着没有空隙)   如$("p.nav")  选中具有样式nav的p标签

~~~javascript
$("#div")
$(".div")
$("div")
$(".div,#div,.container,i")
$("div.mydiv")
~~~

##### 层级

* 选择所有后代   给定祖先元素选择所有后代     $("form input") //选择form子孙下的所有input
* 选择

### 事件

##### .on()绑定

.on(event,selector)  为匹配的元素绑定event事件，参数selector(可选)表示事件委托时的事件源

事件：

* blur

##### .off()移除绑定

.off(event)

##### .trigger()执行事件

.trigger(eventType)  执行匹配元素上绑定的事件eventType

##### 阻止冒泡/默认行为

* event.stopPropagation();  阻止冒泡

* return false; 阻止冒泡与默认事件

  ~~~javascript
   $("#div1").mousedown(function(event){
   	event.stopPropagation();
   });
   
   $("#div1").mousedown(function(event){
    　　return false;
    });
  ~~~


### 数组操作

##### 添加/删除

1.array.push() :在数组尾部添加新的元素，并返回新的数组长度。

2.array.unshift() :在数组头部添加新的元素，并返回新的数组长度。[听说IE浏览器不支持]

3.array.pop() :删除并返回数组最后一个元素。

4.array.shift() :删除并返回数组第一个元素。

