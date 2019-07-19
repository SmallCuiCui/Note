对象  数组  json数据格式  字符串  Math对象  Date对象  内置对象

### 对象

对象：属性和方法的集合

类是对象的抽象，对象是类的实例

js中一切皆为对象。任何东西沿着原型链都能找到对象，实例的__proto__指向构造函数的prototype.prototype上的方法是所有实例共享的

js在es6以前都是对象，没有类的概念（class语法糖也是基于原型的）

##### 对象定义

* var obj={
  ​	name:value,//键值对
  ​	key:value,
  ​	say:function(){}	
  };

* var obj = new Object();//构造函数
  在对象内部函数使用this指对象本身
  删除对象方法或属性：delete obj.name;

##### 属性,方法的调用

* obj.name   //当明确知道属性名，采用点(.)来取属性值
* obj[key]  //key="name"   //如果属性名存在一个变量中采用中括号来取

##### 遍历

采用for-in变量对象，遍历地到key，再根据key取属性值

~~~javascript
var obj = new Object();
obj.name = "xiaohong";//新增对象属性
obj.age = 19;
delete obj.age;//删除属性
for(var key in obj){
	console.log(key);
	console.log(obj[key]);//属性存在变量key中，访问属性值采用中括号
}
~~~

##### 深拷贝/浅拷贝

浅拷贝：拷贝的是对象的引用，当拷贝改变时，原对象也会发生改变

深拷贝：复制对象的属性与属性值来重新生成一个新对象，改变值不影响原对象

###### 深拷贝的实现

* 使用JSON.stringify与JSON.parse进行转换，但是存在弊端，它会忽略undifned

* jquery的方法实现 $extend([boolean],targetObj,Obj)

  * 不传递参数一，直接将Obj的属性合并到targetObj，遇到重复属性Obj属性将覆盖targetObj
  * 参数一为false，遇到相同属性保留targetObj
  * 参数一为true，当属性值是对象时，需要进行递归的合并，再遇到相同的属性名时保留Obj

  ```js
  // 将参数2,3的对象属性，属性值拷贝给参数一，参数一的原始值会发生改变，返回的是参数一
  var object = $.extend([boolean],{}, object1, object2);
  ```

* 使用递归与for-in方法进行拷贝

  ~~~
  
  ~~~


### 数组

##### 定义数组

* 字面量方式  var arr=[];   //空数组
  ​        var arr=[1,2,3];    //数组的typeof为object,arr.length为3

* 构造函数  var arr = new Array(10);//传一个整数参数为数组长度，传小数会报错，传字符串表示一个数组元素
   var arr = new Array(3,2,1,5,4,6)  //传多个参数就是数组元素
    取值：索引arr[3];
    赋值：arr[2] = 1;
    部分元素没有值的数组叫做稀疏数组，未赋值的数组值为undefine

  同一数组中可以有不同类型的数据

##### 遍历

* 一般 for（var i=1;i<arr.length;i++）{}

* for-in (ES5)       //可用于遍历数组，字符串以及对象（属性）

  ~~~javascript
   for(var i in arr){
     	console.log(i);//得到0-length的字符串数值
      console.log(arr[i]);//获取数组值，[]存在隐性类型转换，将字符串的下标值转换为数字型
    }
  ~~~

* foR-of (ES6)     //用于遍历数组以及字符串

  ~~~javascript
  for(var i of arr){
      console.log(i);   //i为数组元素
  }
  ~~~

##### 数组方法(API)

* 连接

  ~~~javascript
  arr1.concat(arr2);//concat()连接两个数组,并返回新数组。在arr1后面连接arr2
  arr1.join();//将数组元素通过特定字符连接，传参为元素之间的连接符，不传时为逗号
  arr1.toSting();//将数组元素连接为字符串，不传参，默认逗号连接
  ~~~

*  删除/添加

  ~~~javascript
  arr1.pop()//删除并返回末尾元素
  arr1.push()//通过传参向数组末尾添加一个或多个元素，并返回新数组长度
  arr1.shift()//删除并返回数组开头元素
  arr1.unshift()//数组开头添加一个或多个元素，并返回数组长度
  
  //下面两个方法会改变原数组。
  arr1.slice(a,b);//返回arr1从下标为a到下标为b的元素，但是不包含b，返回值为数组
  			//b可省略，则截取到数组末尾，a,b可为负值
  arr1.splice(a,b,c,d)//删除一个或多个元素并添加一个或多个元素，返回被删除元素，返回值为数组
  			//从下标a开始删除b个元素，并添加c,d到删除位置
  ~~~

* 排序

  ~~~javascript
  arr1.sort(function(a,b){return a-b;})//将arr1升序排序，参数为比较函数
  arr1.reverse()//将数组元素颠倒
  ~~~

  arr.sort(fun)详解

  * 会操作原始数组，经过操作后原始数组发生变化
  * 默认排序按照字符编码排序
  * 函数返回值小于0表示两个元素不需要交换位置，1表示要用交互位置，0表示相等，实际上<=0等效。

  ~~~javascript
  var arr1 = [14,23,11,6,87,67];
  console.log(arr1.sort());//[11,14,23,6,67,87] 按字符而非数值排序
  
  function sortRule(a,b) {
    return a-b; // 升序
    // return b-a; // 降序
  }
  arr1.sort(sortRule);
  console.log(arr1);//[6, 11, 14, 23, 67, 87]
  ~~~

* 查询

  ~~~javascript
  arr.includes("abc");//判断数组元素中是否存在字符串abc
  ~~~

* ES5新增方法

  ~~~javascript
  arr.indexOf(value);//从左到右查找value在数组中第一次出现的下标位置
  arr.lastIndexOf(value);//从右到左。。。
  
  arr.forEach();//只是遍历数组，没有返回值。
  arr.map(function(item,index){});//返回一个各个元素执行函数之后的新数组
  arr.filter(function(item,index){});//返回一个新数组，内部函数返回一个条件，为真就保留
  arr.some(function(item,index){});//返回一个Boolean数据，只要有元素满足就返回真
  arr.every(function(item,index){});//返回一个Boolean数据，所有都满足才为真
  ~~~

##### 数组去重

​	1、双重for循环
​	2、采用对象的属性不能重复度的特点  (*效率最高)
​	3、Array.from(new Set(arr));    //feom将类数组转成数组，Set默认去重

##### 排序

* 冒泡排序

  ~~~javascript
  for(var j = 0; j < arr.length-1-i; j++){
      // 相邻的两个数比较大小
      if(arr[j] > arr[j+1]) {
          // 交换这两个
          var temp = arr[j];
          arr[j] = arr[j+1];
          arr[j+1] = temp;
  
      }
  }
  ~~~

* 快速排序

* 选择排序

  ~~~javascript
  // 外层循环控制趟数 length-1趟
  for(var i = 0; i < arr.length - 1; i ++){
  	// 再内层循环开始之前假定当前i下标是最小值
  	var minIndex = i;
  	//内层从i开始,到length-1结束
  	for(var j = i+1; j < arr.length; j++){
  	// minIndex跟j对应的值比较
  	if(arr[minIndex] > arr[j]){
  		// 说明minIndex并不是最小值的下标
  		// 只是记录下标,不交换
  		minIndex = j;
  		}
  	}
  // minIndex这个时候存的就是当前趟的最小值的下标
  	var temp = arr[i];
  	arr[i] = arr[minIndex];
  	arr[minIndex] = temp;
  }
  ~~~

##### 数组清空方法

​	arr.length = 0;
​	arr = [];
​	arr.splice(0);

### json数据格式

：数组与对象的嵌套
var h5 = [
​	{
​	    name:"小高",
​	     age:"18"
​	},
​	{},
​	{}
];

### 字符串

##### 定义

* var str1='';
* var str2 = new String("123");
  ​      取字符:str1[2]  //取字符串的第二个字符
  ​	str1.charAt(2)  //取字符串的第二个字符

##### 字符串常见API

```javascript
str1.charAt();  //返回指定下标的字符
str1.indexOf();//检索字符串，返回下标
str1.lastIndexOf();//从后面开始检索字符串，返回下标
str1.replace("","")//检索字符串，参数后者替换前者，返回新字符串，不改变str1

str1.split(",")  //将字符串以,分割为字符串数组。返回为一个新数组
str1.slice(2,4)  //截取下标从2到4的字符，不包括4
str.substring(2,4) //截取下标从2到4的字符，不包括4
str.substr(2,4);  //截取下标从2开始的4个字符

str.toUpperCase()  //将str中的小写转换为大写，并返回新数组
str.charCodeAt(5)  //得到str下标为5的字符的ASCII编码
String.fromCharCode(97)  //得到ASCII码为97的字符，使用String调用此函数
```
##### 字符串比较大小

​	从双方第一个字符开始比较，若比较有结果则返回，若果相等就比较双方第二个字符

### Math内置对象：

##### 属性

Math.PI  圆周率 不能被修改

##### 方法

~~~~javascript
Math.abs()//取绝对值
Math.ceil()  //向上取整
Math.floor()   //向下取整
Math.round()  //四舍五入
Math.pow(a,b)  //返回a的b次方
Math.sqrt()  //返回数的算术平方根，正的那个根
Math.random()  //返回0-1之间的随机小数，包含0不包含1
~~~~

##### 随机数

~~~javascript
//生成a-b之间的随机数
Math.random()*(b-a)+a;
~~~

### Date内置对象

##### 时间戳

指格林威志时间到现在的毫秒数

##### 定义时间对象

* var date = new Date();//得到当前时间的对象

* var date = new Date(2008,12,32) //设置时间为2009,2,1的时间对象

##### 获取时间

~~~javascript
data.getTime()   //获取时间戳
date.getFullyear() //获取年份
date.getMonth() //获取月份0-11
date.getDate()  //获取日期
date.getDay()  //获取星期几0-6，0为周日
date.getHours()   //获取小时
date.getMinutes()  //获取分
date.getSeconds()  //获取秒
~~~

##### 设置时间

```javascript
date.setTime();
date.setFullYear(2008)
date.setMonth()  //设置月份，如果月份错误，会自动调整
date.setMonth(6,0);//设置到7月，但日期为0，则推到上一个月的最后一天，即6月30
date.setDate()  //设置日期，小于等于0往前推，大于当前月份的日期值，往下一个月推
		//没有setDay，不能设置星期几
date.setHours();
date.setMinutes();
date.setSeconds();
```

### js十个内置对象

* Object  js所有对象基类
* Array   数组对象，定义数组属性和方法
* Number  数字对象
* boolean  布尔对象
* String  字符串对象
* Function  函数对象
* Error  错误对象  try()catch()
* Date  日期对象
* Math  数学对象  属性和方法
* RegExp  正则表达式
* json	

