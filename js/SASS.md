#### 基础

sass是css扩展语言，需要编译之后才能使用，不能直接应用于页面。使用将文件夹拖入koala即可进行编译。

sass有两种后缀名文件：

* .sass  不使用大括号和分号
* .scss  使用大括号和分号

#### 导入

@import  "reset";

sass中导入其他sass文件，最后编译为一个css文件

~~~scss
@import "module/base.scss"; /*注意在末尾一定要写分号*/
~~~

#### 嵌套规则

允许将一套css样式嵌套进另一套样式中，内层样式将其外层选择器作为父选择器。

~~~scss
#main{
    color:#333333;
    width:200px;
  
    .redbox{
        height:100px;
    }
    a,span{
        color:red;
    }
}
//编译效果
#main {color:#333333;width:200px;}
#main .redbox{height:100px;}
#main a,#main span{color:red;}
~~~

#### 父选择器&

&代表嵌套规则外层的父选择器，编译之后将替换成嵌套外层的父选择器。

~~~scss
#main{
    color:black;
    a{
        color:red;
        &:hover{
            color:yellow;
        }
    }
}
//编译为：
#main{color:black;}
#main a{color:red;}
#main a:hover{color:yellow;}
~~~

&可作为选择器的第一个字符，其后可跟后缀生成复合的选择器

~~~scss
#main{
    color:black;
    &-side{color:red;}
}
//编译为
#main{color:black;}
#main-side{color:red;}
~~~

#### 属性嵌套

部分属性存在相同的命名空间，如font-family,font-size,font-weight; background类，可以嵌套

~~~scss
.main{
    font:{
        family:fantasy;
        size:30px;
        weight:bold;
    }
}
//编译为
.main{font-family:fantasy;font-size:30px;font-weight:bold;}

//命名空间也可以包含自己的属性值
.main{
    font:30px {
        family:fantasy;
        weight:bold;
        }
}
~~~





#### 变量

##### 定义$

以$开头。后面加上! default代表是当前变量的默认值

变量支持块级作用域，嵌套规则内定义的变量只能在嵌套规则内使用（局部变量），在嵌套规则外定义的变量可以在任何地方使用（全局变量。添加!global可将局部变成全局变量

~~~scss
//代码注释
/*代码注释*/
$font-size:16px;
div{
    font-size:$font-size;
    $width:5em !global;
}
p{
    width:$width;
}
~~~

##### 复杂变量

定义：$linkColor:#333333 white #cccccc!default; 变量分别作为属性值与变量名使用

~~~
//变量当属性值使用
$linkColor:#333333 white #cccccc!default;
div{
    color:nth($linkColor,2);
}
//编译后
div{color:white;}
~~~

~~~
//#{$name}来使用变量名
$name:top !default;
.box{
    border-#{$name}:1px solid #red;
}
//编译后
.box{
    border-top:1px solid red;
}
~~~

##### 多值变量map与list

是变量多维数据的存储方式，list类似于js数组，map类似于js中的对象

* list

~~~scss
//定义
$list:20px 30px 50px;
$list:(20px 30px)(30px 50px)(54px 25px);//类似于多维数组

nth($list,$n)//返回索引的项目
set-nth($list,$n,$list)  //设置$list的第n个
length($list)//回list的长度

join($list1, $list2, [$separator])//表链接在一起
append($list1, $val, [$separator])//一个值到列表最后
zip($lists…)//个列表组合成多维列表
index($list, $value)//一个列表中值的位置
~~~

* map

~~~scss
//定义
$map:(key:value,h1:200px,h2:300px,h3:500px);
//取值
map-get($map,$key)；//返回key的值

map-merge($map1, $map2) //合并两个$map；
map-remove($map, $keys…) //删除某个value并返回value值；
map-keys($map) //以list形式返回所有$map 的key;
map-values($map) //以list形式返回所有$map中的value;
map-has-key($map, $key) //查看当前的$map是否有这个key
keywords($args)  //返回一个关键字
~~~

~~~scss
//map遍历
$header:(h1:30px,h2:40px,h3:50px);
div{
    @each $key,$value in $header{
    #{$key}{
        font-size:$value;
    }
  }
}
//编译后
div h1 {font-size: 50px; }
div h2 {font-size: 40px; }
div h3 {font-size: 30px; }
~~~

#### @at-root跳出选择器嵌套

默认会跳出选择器嵌套，即变成最外层的样式.

但是无法跳出@media与@support,这两种需分别使用@at-root(without:media),@at-root(without:support)

without关键字取值有4个：rule(表示常规css，默认),all(表示所有),media,support(还未广泛使用，忽略)

~~~scss
/*******/
.box1{
    color:red;
    .box2{
        color:green;
    }
}
//编译后
.box1{color:red;}
.box1 .box2{color:green;}

/***@at-root****/
.box1{
    color:red;
    @at-root.box2{
        color:green;
    }
}
//编译后
.box1{color:red;}
.box2{color:green;}

/****@media下@at-root无法跳出media嵌套***/
@media screen and (max-width:640px){
    .box1{
        color:red;
        @at-root .box2{
            color:green;
        }
    }
}
//编译后
@media screen and (max-width: 640px) {
  .box1 { color: red; }
  .box2 {color: green; } 
}

/****@at-root(without:media)带着父级跳出media嵌套***/
@media screen and (max-width:640px){
    .box1{
        color:red;
        @at-root（without:media){//参数为media时：.box2带着父级.box1跳出了media嵌套
            .box2{
            	color:green;
        	}
        }
    }
}
//编译后
@media screen and (max-width: 640px) {
  .box1 { color: red; }
}
.box1 .box2 {color: green; } 

/***@at-root(without:all)不带父级跳出media嵌套****/
@media screen and (max-width:640px){
    .box1{
        color:red;
        @at-root（without:all){//参数为all时：.box2带着父级.box1跳出了media嵌套
            .box2{
            	color:green;
        	}
        }
    }
}
//编译后
@media screen and (max-width: 640px) {
  .box1 { color: red; }
}
.box2 {color: green; } 
~~~

~~~

~~~







