# js二阶段

### Tips

* 通过方法获取到dom元素的类数组,如li标签的集合lis,遍历时lis[i].index=vlaue与
* var arr = arr1.concat(arr2);  //将数组arr2的元素添加到arr1后生成新数组
* arr.includes(value)判断arr数组是否包含value值,是则返回true，不包含则返回false
* 通过ele.来为元素添加自定义属性时元素查看时不显示的，也可以通过.来获取，
* 通过ele.setAttribute(name,value)会显示在DOM上，获取值时需要用getAttribute("name")获取
* ele.classList.add('ac');//给class添加'ac'，不会影响原有class，兼容IE8

##### 函数相关

* 参数对象arguments，保存函数的所有参数
* arguments.callee属性，返回正在执行的函数本身
* fun.caller  函数的属性，返回调用函数的函数，若函数未执行或调用者不是函数，则返回null



### ES5新增

##### ES5严格模式

区别于”正常模式“，使js在更严格的条件下运行，部分在正常模式下能运行的语句，在严格模式下不能

兼容性：IE10以内的主浏览器都支持

进入严格模式："use strict"

~~~html
<script>
	"use strict"
</script>
<script>
    function fun(){
        "use strict";
        console.log("函数进入严格模式");
    }
</script>
~~~

严格模式行为变更

* 全局变量声明时必须加var

* this无法指向全局对象。正常模式下全局作用域下的this指向window

* 函数内不允许存在重名参数，正常模式下可以，但是后者覆盖前者

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

* Array.from(lis);  将类数(如元素节点集合NodeList)转换为数组

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
var a = 20;//变量是的作用域范围以函数为边界（函数作用域）
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
  //ar {name,ag="sdc"}=obj;
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
  ~~~

##### 字符串处理

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

##### Symbol()生成状态标志

Symbol()函数会生成一个唯一的值，作为一些字典变量的值

~~~javascript
var obj = {
			red:Symbol(),
			green:Symbol(),
			yellow:Symbol(),
			orange:Symbol()
		};
	//用于某些需要标记的地方，不需要自己设置不同的值来进行标记，使用Synbol自动生成唯一值
~~~

##### Map()与Set()

属于一种集合，本质上是对数组的包装,遍历采用for-of，没有提供下标的访问方式

~~~javascript
//Set()使用
let imgs = new Set();
imgs.add(1);//Set集合是默认去重复的，随后再加入1时不会重复加入
imgs.add("sgdh");
//for(var item of imgs.values())//遍历得到值
//for(var item of imgs.keys())//遍历得到名，与imgs.values()结果一样
//for(var item of imgs.entries())//得到的值是数组[1,1] ["sgdh","sgdh"]
for(var item of imgs){//遍历方式，采用下标不能获取到值
    console.log(item);//1 sgdh
}
~~~

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
class person{
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    say(){
        alert(this.name);
    }
}
var xiaoming = new person("xoapming",18);
~~~





