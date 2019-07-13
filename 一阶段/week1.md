### Tips

#### 抠图

截图标方法：矩形选框工具选中目标 ctrl+C ctrl+N(新建) ctrl+V 存储为web文件

打开文件夹：Ctrl+O

打开标尺：Ctrl+r

表示路径:URL,src,hrf

### HTML基础

建站流程：注册域名，租用空间，网页制作，网站推广，网页维护
W3C万维网联盟：制定结构和表现标准

H标题：居中属性align="center"

\<hr/>水平线

\<br/>强制换行

排列方式：横向排列，双标签

加粗：strong b

倾斜：em i

字体：font  拥有属性size1-7  color

div标签

span标签：文本结点，横向排列，没有任何默认样式

调试：快捷键f12或者鼠标右键审查元素

CSS层叠样式表，表现标准语言

XML是一种数据格式，用于前端和后台数据使用，它的标签可以自定义

json是一种数据格式，比xml更好用，取代xml

### 列表ul ol dl

自上而下排列

1. 无序列表：ul  li   ~常用
2. 有序列表：ol  li    type属性：I,a,A,数字，默认值为数字    start属性:开始位置,针对数字起作用
3. 自定义列表：dl  dt（名字/图片） dd（解释）

* 去掉无序列表前面的小点（列表符号）list-style:none;
* 文本垂直居中：行高等于容器高度时可以实现文本垂直居中

### 超链接a

a 横向排列

* 属性：href (路径),  target="_blank"新窗口打开,
* 空链接可返回顶部，不能设置宽和高，常设置为href="javascript:;"避免回到顶部
* 超链接a的颜色不能继承父元素的样式，其他元素可以继承
* 去除a链接下的横线text-decotration:none;

### 图片img

img  横向排列  属性：src(路径),width,height,title.alt(无法加载时提示信息)

#### 路径类型：

##### 相对路径 
1. 当前文件与目标文件在同一个目录下面，`文件名.扩展名`

2. 当前文件与目标文件所在文件夹在同一目录下面，`文件夹名/目标文件名.扩展名`

3. 当前文件所在文件夹与目标文件所在文件夹在同一目录，`../目标文件所在文件夹名/目标文件.扩展名`

   （../表示在当前文件返回上一级文件夹）

##### 绝对路径

（带盘符的路径） 如 E://练习//2day5//images//mm1.jpg

* 图片在段落中垂直居中：css:vertical-align:middle   html：valign="middle"

### 表格table

：table  tr(行),td(列).th(表头)，caption（表格标题的标签，写在table内部开头位置）

#### html属性

* border边框

* height，width

* cellspacing单元格之间的间距，

* cellpading内容到边框四周的距离，两个属性常清零

* align 水平对齐方式：align="left/center/right"  设置在表格(表格)上或者tr（行）上或在td（单元格,不是列）上

* valign 内容垂直对齐方式：valign=“top/bottom/middle"  设置在tr或td上，不能设置在table上

* bgcolor：背景颜色 bgcolor="red"

* bordercolor：边框颜色 只能给table设置

* `colspan`:合并列 在当前行colspan="2",再删掉多余单元格

* `rowspan`:合并行  （ 先合并列，在合并行）

   设置表格高宽之后，在单元格填入内容可能导致单元格高宽变化，需单独设置该行列的高宽
     ?     表格布局过去常用，现在应用较少，多用div和css布局

#### css属性

* table, th, td 单独设置宽高，背景，颜色，等

* vertical-align 属性设置垂直对齐方式，比如顶部对齐、底部对齐或居中对齐：

* text-align 属性设置水平对齐方式，比如左对齐、右对齐或者居中：

* border-collapse 属性设置是否将表格边框折叠为单一边框

  * separate  分开，默认值
  * collapse 合并

* border-spacing  设置分隔单元格边框的距离

* caption-side  设置表格标题的位置  top,bottom,inherit

* empty-cells  设置是否显示表格中的空单元格  hide,show,inherit

  ~~~css
  //表格常用样式
  table{
      border:1px solid;
      border-collapse: collapse;
      border-spacing: 0;
  }
  ~~~


### 表单form

form表单标记  自上而下排列。作用：收集用户信息给服务器

#### 属性

* name属性：表单名称
* method属性：表单提交方式  get方式（不安全） post方式   不写默认值为get
* action属性：后台地址
* target属性:新窗口打开

#### 表单框

从左到右（横向）排列

##### input标记

\<input type="text" name="username"/>

* name属性：名称，表单提交之后会作为参数传递到后台
* type属性：表单类型，属性值不同决定表单框的功能不同

  * checkbox：复选框

  * submit:提交按钮，数据提交到服务器，区别button标签按钮，它常用于跳转

  * text：文本输入框

  * file：文件上传

  * reset：重置按钮，在不刷新的页面的情况下清空内容

  * password：密码输入框

  * radio：单选按钮，需要设置name   1)同一组name值必须设置，且必须一样 2)不同组的name不能相同

    ~~~html
    <input type="radio" name="sex"/>男<input type="radio" name="sex"/>女
    ~~~
* value：初始值
* checked="checked"默认选中
* disabled="disabled"禁用，常用于输入电话号码之前的获取验证码按钮
* placeholder：提示信息，当输入内容时会消失

#### 选择select

从左到右排列  用于指定数量的情况下  selected默认选中

~~~html
<select>
    <option>1899</option>
    <option selected>1990</option>
    <option>1992</option>
</select>
~~~

#### 文本区域textarea

属性：cols一行字符数，rows行数，如果输入行数超过，会自动出现滚动条

~~~html
<!--form表单-->
<form name="login" method="post" action="http://www.baidu.com" target="_blank">
     用户名：<input type="text" value="请输入用户名"/>
     密码:<input type="password" placeholder="请输入密码”/>
     <input type="submit" value="注册"/>
</form>
~~~

### CSS样式表

CSS样式表，放置css代码的环境，3类

#### 内部样式表   

适用于代码量较少 style标签（type定义文档类型）\<style type="text/css"></style>

#### 外部样式表 

适用于代码量较多   

* link标签

     rel关联样式表 由html提供，兼容性好，结构和样式同时加载

    ~~~html
    <link rel="stylesheet" type="text/css" href="。。"/> 
    ~~~

* 使用import

   由css提供的方式，兼容性不好，js无法控制，网页加载的时候先加载结构后加载样式

  ~~~html
  <style type="text/css"> @import url(css/style.css); </style>
  ~~~

#### 内联（行内）样式表 

偶尔使用    写在标签内部   优先级最高

 内部，外部样式表的优先级与书写顺序有关，后书写的覆盖先书写的样式，优先级覆盖的是相同属性的样式，不同属性的样式

### CSS选择器

CSS语法：选择器+声明（声明分为属性和属性值，由冒号链接，分号结束）

选择器（选择符）：选择符表示要定义样式的对象，可以是元素本身，也可以是一类元素或自定义名称元素

1. 标签选择器，通过标签名选择
2. class选择器   class="name1  name2 ..."可有多个类名
3. id选择器
4. 包含选择器（后代选择器）  可以限制样式的范围 权重为各选择器之和
5. 群组选择器   一次性有范围的选择多个  标签1,标签2,标签....{} 注意点：最后一个标签不能写逗号
6. 通配符选择器 *{}   选中所有标签 重置样式，把默认样式清掉重新设置  *{margin:0;padding:0;} 
7. 伪类选择器：超链接的一种  顺序最好不要乱 L V H A
   * a:link 初始状态
   * a:visited  访问过后的状态  刷新之后不会回到初始状态  鼠标访问过后的状态，浏览器有缓存问题，只有				把缓存清除掉才能回到初始状态。
   * a:hover  鼠标悬停时的状态 **使用最多，可以给其他标签使用
   * a:active  鼠标点击时的状态	

选择器权重：内联1000   id：100     class：10     标签：1
?	优先级的权重，样式表是优先级，选择器叫权重，当发生冲突时，高的覆盖低的。
?	就近原则

### css常用属性

##### margin padding

* margin：外边距,元素与元素之间的距离    不会撑大该容器的大小
  ?	让自上而下排列的元素可以水平居中 margin:0,auto; 
  ?	margin上下的值会发生重叠，左右不会
* padding：内边距 内容到边框的距离，会撑大该容器的大小



##### 行高 line-height

行与行之间的距离，测量：一行文字的顶部到下一行文字的顶部及改行文字的行高。
?	1)当行高大于容器高度时，文本垂直往下
?	2)当行高小于容器高度时，文本垂直往上
?	3)当行高等于容器高度时，文本居中**

##### 水平对齐方式：text-align
?	center,left,right,justify(两端对齐)

##### 背景：background-color
?     背景图 background-image:url(images/1.jpg);
?	1)当背景图>容器大小，背景图某些部分显示不完整 从背景图左上角开始进行显示
?	2)当背景图<容器大小，默认背景图平铺
?	3)当背景图=容器大小，刚好显示
?     背景平铺属性 background-repeat
?	repeat: 平铺 默认
?	no-repeat 不平铺
?	repeat-x  横向平铺
?	repeat-y:纵向平铺
?     背景图位置属性 background-position:left/right/top/bottom/center/具体值
?	background-position:left center;
?	background-position:10px 20px;
?	background-position-x:10px;

##### float浮动

float浮动   left/right/none   定义网页中的其他文本如何环绕该元素显示  让至上而下的元素的水平排列

###### 概念

1、标准流(文档流)：正常的网页排版顺序，从上而下的排列与横向排列。
2、浮动流：设置了浮动属性的元素
3、脱离文档流：设置了float属性后元素就从原来的标准流中脱离出来，成为浮动流。
（原先是标准流时要遵循它的一些规范，成为浮动流后不遵循标准流中的规范）
4、元素成为浮动流之后，就像从标准流中删除一样，是漂浮在当前标准流之上
5、成为浮动流之后原先的位置发生了变化    

###### 注意事项

1、两个元素，第一个设置浮动，第二个不设置，第一个会把第二个覆盖掉
2、两个元素，第一个不设置浮动，第二个设置，这时两个元素的位置保持不变
3、两个元素都设置浮动，当宽度够时两者会排在一行，当宽度不够时第二个被挤到第二排
4、浮动元素对文本有吸引力 （ 阻止文本围绕浮动框clear:both/left/right/none）

?	若容器内仅装有浮动元素，因为浮动元素脱离了文档流，所以该包围浮动元素的容器不占据空间，
?	解决方法：1）该容器也浮动 2)容器加一个空元素并且将它clear:both

##### 标准流中 html分3类
###### 块级元素

特点:没有设置宽时，充满父容器，默认独占一行，自上而当下排列，可以设置宽高   div,p,h1-h6,ul,li,table,form  脱离文档流(浮动，绝对定位，固定定位)之后不独占一行，宽度由内容大小或宽度决定

###### 行内(内联)元素

特点：横向排列，不能设置宽高 em,b,strong,i,span,a...当设置浮动或者变成块元素之后才能设置宽高

###### 行内块元素

特点：横向排列的块，可以设置宽高    input,select,img
?	浮动流为了解决排版问题-让自上而下的元素横向排列，内联元素设置浮动之后也可以设置宽高

导航：浮动float，line-height,width

##### 网页制作相关
当容器没有设置高度的时候，图片会默认把容器底部撑大2px;
input默认有2px的边框，清除默认边框border:none;   border:0;
input，img标签换行会在标准流中解析出空格，解决方法：input加浮动
谷歌浏览器下最小字体支持12px;
容器内加图片占位置，背景图不占位置










?	