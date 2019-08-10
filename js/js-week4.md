ES5  ES6   面向对象  this指向问题

### Tips

* 通过方法获取到dom元素的类数组,如li标签的集合lis,遍历时lis[i].index=vlaue与
* var arr = arr1.concat(arr2);  //将数组arr2的元素添加到arr1后生成新数组，不改变原数组
* arr.includes(value)判断arr数组是否包含value值,是则返回true，不包含则返回false
* 通过ele.来为元素添加自定义属性时元素查看时不显示的，也可以通过.来获取，
* 通过ele.setAttribute(name,value)会显示在DOM上，获取值时需要用getAttribute("name")获取
* ele.classList.add('ac');//给class添加'ac'，不会影响原有class，兼容IE8

##### 函数相关

* 参数对象arguments，保存函数的所有参数
* arguments.callee属性，返回正在执行的函数本身
* fun.caller  函数的属性，返回调用函数的函数，若函数未执行或调用者不是函数，则返回null

##### 函数节流/函数防抖

* 函数节流：一般在scroll里面，只是在结束滚动的时候触发一次函数。常采用isScroll标记

  ~~~javascript
  var timer = null;
  var isScroll = false;
  window.onscroll = function(){
      isScroll = true;
      clearTimeout(timer);
     //当一直在滚动时都会触发Scroll，清除定时器，只有停止Scroll之后延时计时器timer内的内容才能执行
      timer = setTimeout(function(){
          isScroll = false;
          //执行行为
      },50)
  }
  ~~~

* 函数防抖：一般在定时器用，在打开下一个定时器之前先把上一次的定时器清除

  timer定义为全局，每次进入定时器先清除定时器

### ES5新增

##### ES5严格模式

区别于”正常模式“，使js在更严格的条件下运行，部分在正常模式下能运行的语句，在严格模式下不能

兼容性：IE10以内的主浏览器都支持

进入严格模式："use strict"

~~~html
<script>
    //全局使用严格模式
	"use strict"
</script>
<script>
    function fun(){
        //局部使用
        "use strict";
        console.log("函数进入严格模式");
    }
</script>
~~~

严格模式行为变更

* 全局变量声明时必须加var

* this无法指向全局对象。正常模式下全局作用域下的this指向window

* 函数内不允许存在重名参数。正常模式下可以，但是后者覆盖前者

* 函数参数对象arguments不能被动态改变

  ~~~javascript
  function fun(a){
      "use strict"
      a = 10;
      console.log([a,argument[0]]);
      //严格模式下输出[10,1]
      //正常模式下输出[10,10]
  }
  fun(1);
  ~~~

* 函数参数对象的属性arguments.callee与函数属性fun.caller不能使用

* 新增保留字：inplements,interface,let,package,private,protected,public,static,yield

##### 数组方法

###### 2个索引方法

* arr.indexOf(value)//返回数组中从左开始value第一次出现时的下标，若不存在则返回-1
* arr.lastIndexOf(value)//返回数组中从右开始value第一次出现时的下标

###### 5个迭代方法

* forEach() 遍历数组

~~~javascript
var arr = [1,5,4,7,2];
arr.forEach(function(item,index){
    //item为数组元素，index为索引
})
~~~

* map()  返回一个新数组，数组中的元素为原始数组元素调用函数处理的后值。 

  注意如果数组中的元素是对象时，当使用map改变过元素时，原数组对象也会被改变，因为对象是引用类型。

  ~~~javascript
  var arr1 = arr.map(function(item,index){
      return item*2;//新数组每一个值是原数组值得两倍
  })
  ~~~

* filter()  返回新数组

  ~~~ javascript
  var arr2 = arr.filter(function(item){
      //返回值为true的话这个值会保留
      return item>10;
  })
  ~~~

* some()  返回一个boolean值

  ~~~javascript
  arr.some(function(item,index){
  	consol.log(item)
  	//只要有一个满足item条件就会返回true，并且不会再往后面去查找
      return item >10;
  })
  ~~~

* every()  返回一个boolean值

  ~~~javascript
  arr.every(function(item,index){
  	consol.log(item);
      return item > 10;
  })
  ~~~

###### 2个归并方法

* reduce()  前一次的返回值作为下一次的prev， 数组元素从前往后迭代（计算数组元素累加和，去重）

~~~javascript
var arr = [2,3,45,28,45];
var sun = arr.reduce(function(prev,next){
    //将前一次的返回值做为下一次的prev，next为数组中的每一个数
    return prev+next;
},0);//参数0表示第一次进入时prev=0;不要参数时prev为数组第一个元素
~~~

~~~javascript
//数组去重
//arr.includes(value)返回boolean值，判断arr中是否有value值，全等匹配
//arr.index(value)返回arr中value值得下标，没有的话返回负一
var arr = [2,3,2,2,8,45,28,45];
var arr1 = arr.reduce(function(res,next){
    //新数组如果没有当前值就push
    if(!res.includes(next))res.push(next);
    return res;
},[])
~~~

* reduceRight() 类似reduce，元素迭代是从后往前的

  ##### JSON

  前后端数据要进行传输的话，属性名必须用双引号引起来

  var arr = JSON.parse(str)  把''满足json格式的字符串''转成json  (如果字符串不满足会报错)

  var str = JSON.stringfy(josn)  将json转换为满足json格式的字符串

##### 字符串方法

* str.trim()  去掉字符串前后的空格

  ​	str.trimLeft()去掉左边空格，str.trimRight()去掉右边空格

* str.replace(reg,"");

##### 日期对象Date

​	Date.now();  //获得当前日期的毫秒数，不需要新new Date

##### 对象Object的方法

* Object.defineProperty(obj,'name',{}) 给obj设置属性

~~~javascript
var obj = new Object();
Object.defineProperty(obj,{
    name:{
        value:'张三',
        configurable:false,//表示能否通过delet删除此属性
        writable:true,//能否修改属性的值
        enumerable:true //表示此属性是否可以通过for-in,Object.keys()来枚举
    },
    age:{
        value:18
    }
})
~~~

* var arr = Object.keys(obj)  获取obj的所有属性名称，返回一个数组。对象返回属性名，数组返回下标
* var arr = Object.values(obj)  获取obj的所有属性值，返回一个数组。对象返回属性值，数组返回数组元素
* Object.assign(obj1,obj2);对象合并,属性冲突时后面的覆盖前面的。数组合并时，下标相同的后者覆盖前者

### ES6

##### 添加方法

* Array.from(lis);  将类数组(如元素节点集合NodeList)转换为数组

##### 声明变量

* var a; 函数作用域，存在变量提升

* let b;块级作用域，没有变量提升，有预编译

  let有预编译：在它的块级作用域内外都有同一个变量时，优先找自己的，但是在定义前使用会报错(没有变量提升)

* const c=10;声明常量，不允许修改，修改会报错。声明对象/数组时，对象本身不能被修改，但是可以修改对象属性

  引用类型可以修改内部结构，但是不能重新赋值为新的对象或数组

~~~javascript
//对比var与let作用域的区别
consol.log(a);//undefine
consol.log(b);//会报错
var a = 20;//变量的作用域范围以函数为边界（函数作用域）
let b = 3;//作用域范围以块为边界，代码块即花括号包起来的（块级作用域），不存在变量提升
if(a>10){
    console.log(b);//报错，暂时性死区，在自己的块级作用域内有b存在，不会去找全局的b，
    let c =1;
    let b = 2;
    console.log(c);//1
}
console.log(c);//报错

~~~

~~~javascript
//对应下标获取
for(var i=0;i<list.length;i++){
    list[i].click = function(){
        console.log(i);//结果全为list.length，得不到下标
    }
}

for(let i=0;i<list.length;i++){
    list[i].click = function(){
        console.log(i);//可以得到对于下标，因为每次循环都是不同的块级作用域
    }
}
~~~

##### 扩展运算符(...)

... 功能是把数组，或类数组对象扩展成一系列以逗号隔开的值

~~~javascript
//数组元素作为函数参数
var arr = [1,5,4,2,5,8];
console.log(...arr) //得到1 5 4 2
function fun(a,b,c,d){
    console.log(a,b,c,d);
}
fun(arr[0],arr[1],arr[2],arr[3]);//传统写法
fun(...arr);//使用扩展运算符传参,数组多余部分传入参数不影响，保留在arguments中

//进行数组拼接，或对象属性
var arr1 = [...arr,6];//[1,5,4,2,5,8,6]
var obj = {"name":"jsi"};
var obj1 = {...obj,"age":18} //{"name":"jsi","age":18}
~~~

##### rest运算符(...)

功能是将多个逗号隔开的元素合成数组

~~~javascript
function fun(...arr){
    console.log(arr);
}
fun(4,5,8,2)
~~~



##### 模板字符串

``相比较于' '，模板字符串不怕换行，可以通过${}使用变量，可以使用函数

~~~javascript
var json=[{name:"xiao",age:12},{name:"xiao",age:12}];
var ul = document.querySelector('ul');
var html="";
json.forEach(function(item,index){
    html +=`//此字符为esc键下的那个，不是单引号
	<li>
		<span>${item.name}</span>
		<span>${item.age}</span>
		<span>${item.age>20?"大龄":""}</span>
	</li>
`
});
ul.innerHtml = html;

//普通字符串(单引号或双引号)害怕换行，每行都需要引号
/* html += '<li>'+
			'<span>'+ shop.name +'</span>'+
			'<span>'+ shop.num +'</span>'+
			'<span>' + shop.price + '</span>'+
	'</li>'; */
~~~

##### 箭头函数

使用=>声明函数

~~~javascript
var fun = function(){
    alert("fun");
}
var fun = () =>{//函数没有参数
    console.log("fun");
}
var fun = (a,b) =>{//括号内为参数
    console.log(a,b);
}
var fun = a =>{//fun只有一个参数的时候可以不要括号
    console.log(a);
}
//函数只有一句，且这一句就是return，则可以省略大括号和return
var fun = a =>a+1;//console.log(fun(3)) //4
//函数返回对象

~~~

箭头函数没有自己的this，自动指向外围

~~~javascript
//箭头函数没有自己的this，自动指向外围
var name=10;
var obj = {
    name:"xiaohong",
    say:function(){
        console.log(this.name);//xiaohong  this指向obj
    },
    say1:()=>{
    	console.log(this.name);//输出为window
        console.log(this.name);//10  this指向外围的
    }
}
obj.say();
obj.say1();
~~~

##### 解构赋值

* 数组解构，将数组的值按顺序解构出来

  ~~~javascript
  //数组解构用[]
  var arr = [2,3,4，5,6];
  var [a,b,c]=arr;//解构数组前3个
  console.log(a,b,c);//2,3,4
  ~~~

* 对象解构,变量名必须与对象属性名对应，否则未定义，不一致的变量名可以赋初值

  ~~~javascript
  //对象解构用{}
  var obj = {name:"xiao",age:20};
  var {name,ag}=obj;
  //var {name,ag="sdc"}=obj;
  console.log(name,ag);//name="xiao"  ag：undefined
  
  //对象与数组一起解构
  var json = [
      {username:"xiaohong",age:20,arr:[2,3,4]},
      {username:"lisi",age:20,arr:[2,3,4]},
      {username:"liuneng",age:20,arr:[2,3,4]},
  ];
  var [p1,{username}]=json;
  console.log(p1);//{username:"xiaohong",age:20,arr:[2,3,4]}
  console.log(username);//lisi  对应数组第二个对象的username
  
  //对象
  var name = "xiao",
  	age = 18;
  var obj = {name,age};
  console.log(obj);//一个对象
  ~~~

* 解构赋值与扩展运算符一起使用

  ~~~javascript
  var obj = {
      name:"zhangsan",
      age:18
  }
  var id = 15;
  
  obj = {...obj,id}
  ~~~


##### 字符串处理方法

* 字符串的Unicode表示 规则：\u+四位16进制

  ~~~javascript
  console.log("\u0061");//a
  //只能表示\u0000~\uffff之间的字符，超出范围必须使用双字节表示
   console.log("\uD842\uDFB6");//𠮶
  ~~~

* str.repeat(key)//字符串重复key次拼接在一起，返回新字符串

* str.includes("str1");  判断str字符串中是否含有str1字符串，返回boolean型

* str.startWith("str1");判断字符串开头是否为str1，返回boolean型

* str.endWith("str1");判断字符串结尾是否为str1，返回boolean型

* for-of遍历字符串中的每一个字符

  ~~~javascript
  var str = "shgduw";
  for(var i of str){
      console.log(i);//s h g d u w
  }
  ~~~

##### Symbol()类型

第六种基本数据类型Symbol。Symbol()函数会生成一个唯一的值(可用于状态标志)，作为一些字典变量的值

~~~javascript
var obj = {
			red:Symbol(),
			green:Symbol(),
			yellow:Symbol(),
			orange:Symbol()
		};
	//用于某些需要标记的地方，不需要自己设置不同的值来进行标记，使用Synbol自动生成唯一值
~~~

##### 数据结构Map()与Set()

属于一种集合，本质上是对数组的包装,遍历采用for-of，没有提供下标的访问方式。使用前需new一个实例

###### set()

类似于数组，但数组允许有重复元素，Set不允许元素重复

方法：

* size:获取元素数量
* add(value):添加元素，返回Set实例本身
* delete(value)：删除元素，返回Boolean值，表示是否删除成功
* has(value)：返回Boolean值，表示该值是否是Set实例的元素
* clear()：清除所有元素，没有返回值

~~~javascript
//Set()使用
let imgs = new Set();
imgs.add(1).add(2).add(2);// [1,2] Set集合是默认去重复的，不会重复加入
//for(var item of imgs.values())//遍历得到值
//for(var item of imgs.keys())//遍历得到名，与imgs.values()结果一样
//for(var item of imgs.entries())//得到的值是数组[1,1] ["sgdh","sgdh"]
//imgs.forEach((value,key)=>{})
for(var item of imgs){//遍历方式，采用下标不能获取到值
    console.log(item);//1 2
}
~~~

###### map()

类似于普通对象，但是普通对象的key值必须是数字或字符串，而Map的key可以是任意数据类型.

Map 实例的属性和方法如下：

- `size`：获取成员的数量
- `set`：设置成员 key 和 value
- `get`：获取成员属性值
- `has`：判断成员是否存在
- `delete`：删除成员
- `clear`：清空所有            

~~~javascript
//Map()
var map = new Map();
map.set("name","xiaos");
map.set("age",18);
map.set("name","qing");//去重，将覆盖前面的name值
//for(var item of map){//遍历map中得到item为一个数组，可直接采用解构赋值
//for(var item of map.values())//遍历得到值
//for(var item of map.keys())//遍历得到名
//for(var item of map.entries())//
for(var [key,value] of map){
	console.log(key,value)；//name qing //age 18
}
//获取元素
map.get("name");//qing
    
~~~

##### class 语法糖

定义一个类，然后通过这个类来生成实例

其中constructor被称之为构造方法，在我们new 一个对象的时候，自动被调用，class类必须先定义再使用

~~~javascript
class person{//一个抽象
    constructor(name,age){//构造函数，
        this.name = name;
        this.age = age;
    }
    say(){//语法糖方法默认在原型上，所有实例的函数都是同一个
        alert(this.name);
    }
}
var xiaoming = new person("xoapming",18);//每new一次，构造函数就会被调用一次
~~~

### 面向对象

##### 创建对象的方式

* 字面量 var obj = {}
* 通过new运算符  var obj = new Object()
* 构造函数
* ES6语法糖

##### 构造函数

外形上与其他函数相同，习惯于大写字母开头(小写字母开头也可以)，采用new调用函数，返回一个Object

* 用来构造(创建)对象的函数
* 它在声明的时候和普通函数没有区别
* 用new运算符加上函数调用，调用结果返回一个Object对象
* 构造函数中的this指的是即将new的那个对象

~~~javascript
function Fun(){
    this.name = 'zhang';
    console.log(this);
};
var a = Fun(); //普通调用，Fun是个普通函数
var b = new Fun();  //new调用，Fun是个构造函数，函数中的this指向new生成实例，即b
console.log(a);  //得到undefined，函数没有返回值
cobnsole.log(b);//得到一个Object
~~~

##### 函数原型prototype

是一个指针，指向原型对象（原型是函数的伴生体）

每一个创建的函数都有一个prototype属性，这个属性是一个指针，指向当前对象的原型对象，这个对象包含所有实例共享的属性和方法

~~~javascript
function Fn(name){
    this.name = name;
    this.say = function(){
        console.log(this.name);
    }
}
//构造函数中写属性；方法写在原型上，实例可以共享原型上的方法
Fn.prototype.say1 = function(){
    console.log(this.name);
}
//一次性定义多个函数方法一
Fn.prototype = {
    constructor:Fn,//重新赋值函数的原型，需要使用constructor指回去
    say2:function(){},
    say3:function(){}
}
//一次性定义多个函数方法二
Fn.prototype = Object.assign(Fn.prototype,{
    say2:function(){},
    say3:function(){}
})
var a = new Fn('zhang');
var b = new Fn('heng');
console.log(a.say === b.say);//false，不同实例的方法不一样
console.log(a.say1 === b.say1);//true，都是原型上的函数，原型上的函数是共享的
//实例下的__proto__与构造函数的prototype是一样的，因此实例
~~~

对象上的属性和方法

* constructor 构造函数的原型(prototype)里面的constructor指向构造函数本身

* \_\_proto\_\_ 指向父类的prototype，每一个对象都有这个属性

* prototype  一个指针，指向当前对象的原型对象。只有函数会有这个属性

* instanceof   运算符，判断当前对象是否是另一个对象的实例  

  ~~~javascript
  console.log(tom instanceof Cat);//true  可以用于判断变量是否为数组或对象
  ~~~

* hasOwnProperty  判断实例对象上是否存在某个属性，且这个方法会过滤到原型上的属性

  ~~~javascript
  console.log(tom.hasOwnProperty("say"))//true，构造函数里的属性为true
  //原型上的方法为false
  console.log(tom.hasOwnProperty("name"))sjhdu
  ~~~

* isPrototypeOf  检查对象是否存在于另一个对象的原型链上

  ~~~javascript
  //判断miao是否在b的原型链上
  console.log(Cat.prototype.isPrototypeOf(miao));
  ~~~


##### 原型链

对象都有\_\_proto\_\_属性，指向其构造函数的prototype

构造函数都有prototype属性，是一个指针，指向构造函数的原型对象。构造函数由Funtion()构造出来的实例，也有\_proto\_属性，指向Function的prototype

~~~javascript
function Cat(name){
    this.name = name;
}
var tom = new Cat('Tom');
~~~

![原型链](images\原型链.png)

##### 面向对象的三大特性

* 封装：把一些相关的对象和属性放到一起，用一个变量抽象出来，就完成这个对象的封装

* 继承：子对象可以使用父对象的属性和方法

* 多态：重载，重写

  重载：根据不同参数类型，参数个数实现不同功能

  重写：子对象重新定义与父对象方法名相同的不同方法

### this

##### this指向

* 全局this指向window。匿名函数的this指向window。定时器里的函数常为匿名函数

  全局定义的函数，直接调用时属于在全局调用，this指向window。当赋值调用时，指向所赋的那个对象

* 构造函数的this指向即将new的实例

* 对象方法的this指向对象本身

* IIFE(立即执行函数表达式)  自调用函数的this指向window，不管该自调函数在什么环境下

* 事件里的this指向事件触发对象。

* 箭头函数没有自己的this，自动找外层的this

##### 修改this指向

###### call()/repply()  

函数调用时修改this指向

* obj1.say.call(obj2,key1,key2);

  调用时修改obj1的this指向obj2，方法say属于obj1的.call的第二三个参数对应say函数的参数

* obj1.say.apply(obj2,[key1,key2]);

  同call，但是apply参数传递采用数组

  ~~~javascript
  var obj1 = {
      name:"zhangsan,
      say:function(a,b){
          console.log(this.name);
          console.log(a,b);
      }
  }
  var obj2 = {
      name:"lisi"
  }
  obj1.say.call(obj2,2,3);//lisi,2,3
  obj1.say.rapply(obj2,[2,3]);//lisi,2,3
  ~~~

###### bind()封装时修改this

fun.bind(obj2);

~~~javascript
var obj1 = {
    name:"zahngsan",
    say:function(){
        console.log(this.name);
    }.bind(obj2);
}
var obj2 = {
    name:"lisi",
    say1:function(){
    //更改定时器中匿名函数的this,
	setTimeout(function(){
		console.log(this.name);
    }.bind(this),1000);
    }
}
obj1.say();//lisi
obj2.say1();//lisi
~~~

###### 采用变量留住外层this

~~~javascript
Tab.prototype.bindEvents = function () {
	let _this = this; // 把外层this留住
	btns.forEach(function (btn, i) {
		btn.onclick = function () {
		// 根据当前索引切换图片
		_this.changeImg(i);
		};
	})
}
~~~

