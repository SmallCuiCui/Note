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

* \s    空白字符,空格

* .   全部字符，若要匹配.需要使用转义字符\.

* \b  单词边界

  \B非单词边界

##### 量词

{n}  匹配n次

{n,m}   最少n次，最多m次

{n,}  最少n次，最多不限

\+   {1,}

?    {0,1}

\* {0,}

###  支持正则的字符串方法

* str.search(reg)  查找第一次匹配的子字符串的位置，如果找到就返回一个number类型的index值，否则返回-1

* var str1 = str.replace(reg,"") 返回一个新字符串。 替换匹配字符串，参数一为正则，参数二为替换字符串。

  ~~~javascript
  var str = 'MdvredjfTMD shfwsMMPdsjfhu';
  var reg = /(md)|(tmd)|(mmp)/gi;
  var str1 = str.replace(reg,function(sub){
  	//sub是当前匹配到的字符串
  	console.log(sub);
  	var ss = '';
  	for(var i=0;i<sub.length;i++){
  		ss += "*";
  		}
  	return ss;
  })
  console.log(str1);
  ~~~

* var arr = str.splite(reg);  返回分割数组，参数可以是一个字符或正则表达式。按照参数的内容分割字符串为一个数组

* var arr = str.match(reg)  ; 返回字符串中匹配正则的字符串

### 正则例子

* 固话规则：开头为0,二到三位加-非0开头的八位加结尾一到四位,或是没有结尾

  reg = /^0\d{2,3}-[1-9]\d{7}\d{1,4}?$/

* 电话号码 /^1(3|4|5|7|8)\d{9}$/
* 邮箱

* 只能用数字开头，长度在6--18位之间 /^\d.{5,17}&/

* 匹配首位空格  /^\s+.*\s+$/

* 匹配网址URL：/^http(s)?:\/\/[a-z]{3,5}\.\w+\.\w+(\/.+)*$/9

* 以字母开头，数字结尾，中间任意一个字符/^[a-z].\d&/i

* 密码不能少于6位的字符  /.{6,}/

* 以a开头   b字符至少出现2个，至多出现6个(b连续出现)   /^a\[^b\]*b{2,6}\[^b\]\*/

* 变量的命名正则表达式(不能用数字开头 由字母、数字、下划线 、$组成)   /^[a-z_$]\[\w$\]*$/

* 以010开头的座机号(后面是8位数字)  /^010\d{8}$/

* 手机号以13开头，以8结尾  /^13\d{8}8$/

* 密码只能用6个*   /^\\*{6}$/

* 第一位是数字，第二位是A或a，后面至多出现6个字符    /^\da.{0,6}$/i

* 第一位是数字，第二位是任意一个字符,后面只能由字母、数字、下划线组成，共8位  /^\d.\w{6}$/

* 写出中国人姓名正则    2--4个中文  /^[\u4e00-\u9fa5]{2,4}&/

* 写一个qq号的正则，至少5位  至多12位数字  /^[1-9]\d{4,11}$/

* 邮编检验 共6位数字 第一位不能是0    /^[1-9]\d{5}$/

* 检验压缩包  xxx.zip或xxx.rar或xxx.tar 三个格式   /^\\.(zip)|(rar)|(tar)$/

* 手机号 1 3|5|8|7|9    /^1[35879]\d{9}$/

* 账户名只能使用数字字母下划线，不能数字开头，长度在6--18之间     /^[a-z_]\w{5,17}$/i

* 身份证 （18位  考虑最后一位可能为x）

  /^[1-9]\d{5}((18)|(19)|[23]\d)\d{2}((0[1-9])|(10|11|12))(([0-2]\[1-9])|(10|20|30|31)\d{3}[0-9Xx]$/

