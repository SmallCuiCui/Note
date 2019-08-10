### js预编译

#### 过程（4步曲）

* 创建AO对象，空对象
* 找变量与形参声明，将变量与形参名作为AO属性名，值为undefined
* 将实参与形参统一
* 在函数体里面找函数声明，值赋予函数体。

#### 规则

* 函数声明会置顶，函数名与函数体一起提升

  函数定义采用字面量形式时：var b = function(){}，会当做变量作为提升。

* 变量声明会置顶，只提升声明，赋值留在代码原位置

* 函数声明比变量申明更置顶

* 声明过的变量不会重复声明

#### 案例

案例一：

~~~js
var a = 1;
function b(){
    a = 10;
    return;
    function a(){
        console.log(a);
    }	
}
b();
console.log(a); // 1

~~~

代码分析：根据预解析代码顺序，首先提升整个函数b，然后提升a变量声明，然后a赋值，然后指定b函数，再console；同理函数b内部：提升整个函数a，然后赋值a

执行：当执行b函数时，执行a=10时，由于函数a的存在，在该作用域内存在a变量的地址，故赋值给了函数a，并未影响全局的a，b内的函数a没有调用，也就没有输出。所有最终输出的任然是全局的a=1

案例二：预编译结束之后的AO对象

~~~js
function b(a){
    console.log(a)
    var a = 10
    console.log(a)
    function a(){}
    var b = function(){}
    function b(){}
  }
  b(1)
~~~

分析函数b内的AO：

* 第一步创建AO空对象

  ~~~
  AO = {}
  ~~~

* 找形参与变量，值为undefined

  ~~~
  AO = {
      a: undefined,
      b: undefined
  }
  ~~~

* 实参与形参对应，调用b(1)，故a的值为1

  ~~~
  AO = {
      a: 1,
      b: undefined
  }
  ~~~

* 找函数声明，赋值函数体；找到a函数，变量a被赋值为a的函数体

  ~~~
  AO = {
      a: function a(){},
      b: function b(){}
  }
  ~~~

* 函数以上面的AO为基础执行

  故第一个console为函数体a，第二个输出为赋值后的10

案例三：

~~~js
console.log(a) // a函数体
var a = 10;
function a(){}
console.log(a) // 10
~~~



### dom0与dom2区别

#### 0级DOM

* 标签内使用onclick事件
* js中使用onclick

只能为元素绑定一个处理函数，多次绑定时，后绑定的覆盖前面绑定的。

js中的绑定覆盖元素上直接绑定的。

不包含事件的三个阶段(捕获，目标，响应)

#### 2级DOM

使用监听的方式处理事件addEventListenner() // removeEventListenner()

可以为元素绑定多个处理函数，并且存在事件响应的三个阶段

dom0级与dom2级事件处理可以共存



























