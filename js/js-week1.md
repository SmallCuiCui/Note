变量 数据类型 数据转换 运算符 程序执行结构 函数

### Tips

##### prompt弹框

num=prompt("输入");弹出一个提示输入框，输入数据给num，得到数据是字符型数据
​	得到数字需要转换Number(prompt("输入数字"));

### js基础

##### js概念

JavaScript是一门面向对象的，跨平台的脚本语言

* 对象：属性和方法
* 跨平台：在多个平台都可以运行
* 脚本语言：不能独立执行，需要依赖其他程序。js文件必须嵌入到HTML文件才能执行

##### js组成

- ECMAScript:js标准
- DOM：文档对象模型
- BOM：浏览器对象模型

##### js特点

* 解释性脚本语言
* 运行在浏览器
* 弱类型语言
* 事件驱动
* 跨平台

##### js作用

视觉交互，数据交互，node.js（后台）

### 变量

变量：可变的量

var a = 3;

变量类型:number,string,boolean,undefined,object,function,symbol

##### js命名规范

* 驼峰命名法
* 首字母必须为字母，下划线，或美元符号($)
* 余下字符可以是下划线，美元符号，任何字母，或数字字符
* 大小写敏感
* 避开关键字数据类型

##### 命名第二规则

* 变量名首字符必须为字母(a-z A-Z )，下划线(_)，或者美元符号($)
* 余下字符可以是下划线，美元符号，字母，数字
* 变量名大小写明敏感

### 数据类型

##### 基本数据类型

​	6种：number,boolean,string,undefined,null,symbol(ES6)
​	特点：值在内存中占据固定大小空间，保存在栈内存中，复制时是新创建一个副本，不能给基本类型添加属性

##### 引用类型

​	2种：object(array),function

* 特点：由多个值构成保存在堆内存中，引用类型值的变量实际上包含的不是对象本身，而是指向对象的指针，复制时复制的是指针，因此两个变量指向同一对象，通过其中一个变量改动对象，另一个变量获取的对象值也随之改变，可以为其添加删除属性和方法

* 引用类型的参数传递，同赋值一样

  ~~~javascript
  var person = {
      name:"runtu"
  }
  //引用类型参数传递的是指向对象的指针，在函数内部会更改该对象
  function setName(obj){
      obj.name = "shaonian";
      //在函数内部重新定义引用参数，会变成一个局部对象，函数执行完毕后销毁。不会影响原参数的对象
      obj = new Object();
      obj.name = "nezha";
  }
  setName(person);
  console.log(person.name);//shaonian
  ~~~

##### typeof()的返回

typeof()返回有六种值(变量类型)，返回值均为字符串

* number(Number,NAN)
* boolean
* string
* undfined
* object(对象,数组,null)
* function。

##### instanceof

区分对象与数组，使用A instanceof B，判断a对象是否是b对象的实例

~~~javascript
console.log([] instanceof Array)  //true
console.log({} instanceof Object)  //true
~~~

### 数据转换

##### 隐式转换

​	+号：数字+字母，会将数字转换为字母，并进行字母拼接，输出为字母
​	-/*号：将运算符两边的数据隐式转换为数字类型，并进行减/乘/除运算，输出为数字

字符串减数字，或数字减字符串
​	1、字符串可以转换为数字时，结果为运算结果
​	2、字符串没法转换为数字时，结果为NaN

​	ture+1=2，false+1=1，(true == 2)+1=1，
（true == 2）=false，（true == 1）=true，（false == 2）=false，（false == 0）=true，

##### 显示转换

* parseInt()从左往右开始，遇到特殊字符或小数点就结束
* parseFloat()从左往右开始，遇到特殊字符或第二个小数点就结束
* Number()将只包含数字（整数或小数）的字符串转换为数字类型，不能包含其他非数字字符，否则为NAN
* num.toString()将数字转换为字符串
* String()将数字转换为字符串
* num.toFixed(3)保留num的三位小数，并且转换为字符串***
* NaN不是一个数字的数字，但是typeof(NaN)得到number类型，
  ​	NaN==NaN的判断结果为flase，所有无法采用双等号来判断一个值是否为NaN,采用isNaN(num)判断.

### 运算符

##### 4类运算符

* 赋值运算符(=)
* 算术运算符(+,-,*,/,%)
* 关系运算符(>,<,==,===,!=,!==,<=,>=)
* 逻辑运算符(&&,||，！)

##### 三目运算符

var num = a>b?c:d  先判断a是否大于b，为真则返回c，为假返回b

##### 运算符简写

​	自增a++与++a，console.log(a++),先输出a,后加1。console.log(++a),先+1，后输出a
​		var num=2;   var  res=num++ + ++num + num++;    res=10,num=5;

### 程序执行结构

##### 顺序结构

##### 分支结构（选择结构）

* if(){}else{}判断

  if判断中，所有的数据类型都会被隐式转换为布尔类型

  0、-0、null、""、false、undefined , NaN在if条件里结果为false

  ~~~javascript
  if(){
  }else if(){
  }else{}
  判断句
     1、如果判断条件为赋值语句，则该语句当前值就是当前赋值的值
     2、if括号中只需要bool类型值，所有数据类型都会被隐式转换为布尔类型
     3、0，-0，null,false,undefined,"",NaN作为条件时为false
  ~~~

* switch  case  多分支语句 用于多个确定值需要判断的时候，全等匹配

  注意case穿透，要加break语句（如果程序没有发现break语句，那么解析器会继续向下解析）;

  ~~~javascript
  switch(语句){  //语句的结果与每一条case内容进行匹配，完全匹配
      case 1:
         alert(1);
             break;
          case 2:
               alert(2);
               break;
          case 3:
              alert(3);
              break;
          default:
              alert(0);
  }
  ~~~


##### 循环结构03

循环结构：while,do...while,for

输出一个已定义函数函数名，不加括号时结果为输出这个定义函数函数体。
​	加括号时，若函数有返回值输出返回值，否则会输出undefined(默认返回值)。
​	输出时加了括号，相当于先执行函数再输出。

函数中返回值为n++时，返回的是n，而不是n+1
​	返回值为++n时，返回的才是n+1

循环三大因素
​	1、循环变量初始值
​	2、步进
​	3、终点值

for循环过程 初值，判断，循环体
​	增量，判断，循环体
​	增量，判断，循环体
..........

事件绑定（监听）
​	1、事件监听在html中  <div onclick="calc()"></div>
​	2、on+事件  var box=document/getElementByID("box");
​		box.onclick=fuction(){}或者box.onclick=calc();
​	3、采用element.addEventListener(事件名，函数，冒泡/捕获)


通过element.style只能获取到内联样式



### 函数

封装一段可重复执行的代码（封装，调用）

##### 函数定义

* 函数声明：function  fun(){}
  ​	存在函数声明提升，会被提升到当前作用域的顶部被解析，故函数调用可在函数声明的前面。

* 函数表达式：var fun=function(){}
  ​	函数调用必须在函数表达式之后

##### 立即执行函数

* var fun=function(){}();     //表达式后紧跟括号，函数会立即执行。

* ( function(){…} )() 
*  ( function (){…} () ) 

两种立即执行函数的错误写法
​	1、function(){}();
​	2、function fun(){}();

##### 函数参数

* 形参：函数封装时的参数。
* 实参：函数调用时的参数，实参传递，形参接收
* arguments：实参副本，可以接收函数传的所有参数，函数自带方法。
  ​	选取其中第i个参数采用argument[i];
  ​	获取参数个数arguments.length
* 变量的生命周期：var一个变量时产生，执行环境出栈时释放，如函数内变量在函数执行完毕时销毁

##### 变量作用域

全局作用域   定义变量属于全局变量，在任何地方都可以调用。但是局部变量只能在该局部作用域内使用
局部作用域  函数内部属于局部作用域，可以使用全局变量，并且可以更改全局变量的值。

全局变量：一直存在

局部变量：作用域不存在时被销毁

##### 函数返回

return:将函数执行结果返回函数的调用位置

函数没有return时默认返回值为undifined

##### 预编译

* 检查语法是否有错，有错就不执行

* 变量提升：代码开始执行之前会先将变量的声明提升到当前作用域的顶部。不提升赋值

  在var一个变量之前使用它时不会报错，属于undefined类型。若后面没有var这个变量，则会报错
  只提升声明，不提升赋值。

* 函数提升，是整个函数体一起提升

  var fun=function(){};属于变量提升，在var前调用fun()会报错这个函数未定义。

  function fun(){};在这之前调用则不会出问题，属于函数提升

* 函数与变量名冲突时会保留函数，函数名冲突，后面定义覆盖前面定义

##### 执行上下文

##### 递归

表单中的button标签默认为submit，若想要它为单纯的按钮，添加属性type="button"
表单数据要提交后台的话必须给定name值，没有name值就不提交。默认提交方式为get
html中元素的id名可以在js中直接当变量使用，不需要获取，直接用id名也不会报错，但是不推荐使用。
form在提交之前，常绑定submit事件，来进行数据验证，但是还是会进行提交
alert除了可以弹弹框，还可以暂停程序，直到点击确定