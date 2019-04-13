# 正则表达式

### 定义

* 采用 正则new RegExp(pattern,attributes)构造函数

* 采用/pattern/attributes

  pattern:正则表达式的字符串

  attributes:匹配方式，"g"表示全局匹配，"i"表示区分大小写，"m"多行匹配

  ~~~javascript
  var reg = new RegExp('a');//引号内容为正则
  var reg1 = /b/;//两个斜线之间的内容为正则
  var str = 'sahdeuh';
  reg.test(str);//true
  reg1.test(str);//false
  ~~~

### 使用

##### reg.test(str)

测试某个字符串是否与正则匹配，匹配就返回true，不匹配返回false

##### compile(reg,type)

 reg为正则表达式，type规定匹配类型"g"：用于全局匹配，"i"用于区分下小写,"gi"：用于全局区分大小写的匹配

用于改变或者从新编译正则表达式。被编译过的正则使用效率更高，多次调用的情况下效果显著

##### exec()

返回的是一个数组，数组元素为匹配的子字符串

~~~javascript
var reg = /a/;
var str = "ahtdf";
console.log(reg.test(str));//true
reg.compile('b');//改变正则为reg=/b/;
console.log(reg.test(str));//false
~~~

### 语法

##### 中括号[]

[abshfd]匹配其中的任意一个字符即可

##### 小括号()

用于分组，小括号里面的内容作为一个整体进行匹配

~~~javascript
var reg = /[asdf]/;//匹配asdf中的其中一个
var reg1 = /(ab)|(cd)/;//匹配ab或cd，|表示或
var str = "hfbvuecd";
console.log(reg.test(str));//true
console.log(reg1.test(str));//true
~~~

##### 特殊符号

* |：表示或

* ^：(不放在开头)表示非

* ^：(放在开头)匹配开头

* $：匹配结尾

  /^  $/表示完整匹配

##### 转义字符：元字符

* \d   [0-9]  数字

  \D   非数字

* \w   [a-z0-9_A-Z]数字，字母，下划线

  \W   非数字，字母，下划线

* \s    空白字符

* .   全部字符，若要匹配.需要使用转义字符\.

* \b  单词边界

  \B非单词边界

##### 量词



