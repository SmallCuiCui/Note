npm i react --save

npm i react-dom --save

npm add babel-standalone --save (解析jsx)

新建js文件夹，分别找到依赖包下面umd文件夹下的.js文件放进去

引入文件顺序，react.development.js、react.dom、 babel

~~~
<script type="text/babel"></script>
~~~

语法：

{}，对象无法直接渲染，

### jsx

由babel解析，原理：createElement

#### 基本语法

jsx： JavaScript扩展(js+xml，js代码中使用xml)  语法糖 可以提高开发效率 非必须

class使用要改成className，因为class是关键字

注意script标签的type属性为text/babel

* ReactDOM.render()  参数一直接写一个模板，参数二是模板挂载的元素

  注意参数一的模板的最外围只能有一个标签

* 插值符号{}  

  写变量，

  数组（会将数组内容直接拼接为字符串渲染），

  不能直接写对象，指定写对象的属性

* retrun 后面的内容如果直接换行的话会报错，要么不换行，或者使用()括起来，将它转换为表达式

~~~react
<scrip type="text/babel>
       //  a是一个变量，内容是标签
   var a = <span>react</span>
   var b= 5 
       // 注意对象能直接放在差值符号{}中
   var obj = {b:6}
   ReactDOM.render(<div>hello world {a}{b}{obj.b}<div>,
             document.getElementById('app)
             )
</script>
~~~

#### jsx内注释

{/\*....\*/}

#### jsx与html标签的写法不同处

class->className

vulue->defaultValue

checked->defaultChecked

style要写成对象

label标签的for要写成htmlFor

### react语法基础

####  事件

* 点击事件onClick

~~~react
ReactDOM.render(<div>
                	<button onClick={() => {
    					alert('hello')
					}}>点击</button>
				</div>,
      document.getElementById('app')
    )
~~~

* 键盘事件onKeyUp={}  

  e.keyCode =13 为回车键

* 表单改变事件onChange={}

#### 遍历数组

使用数组的遍历方法arr.map()

~~~react
var arr = [1, 2, 3, 4, 5]
    var colors = ['red','blue','green']
    var colorLis = []
    colors.map(item => {
      return colorLis.push(<li key={item} style={{color: item}}>{item}</li>)
    })
    ReactDOM.render(<div>
                      <ul>
                        {
                          arr.map(item => {
                              return <li key={item}>{item}</li>
                          })
                        }
                      </ul>
                      <ul>{colorLis}</ul>
                    </div>,
                    document.getElementById('app')
    )
~~~

#### 遍历对象

对象转数组：

* Object.keys(obj)   对象键组成的数组

* Object.values(obj)   对象的值转成的数组

* Object.entries(obj)   键值对转成的二维数组，数组的

~~~react
var obj = {a: 4, b: 5}
    ReactDOM.render(<div>
                      <ul>{
                        Object.keys(obj).map((item) => {
                          return <li key={item}>{item}: {obj[item]}</li>
                        })
                      }</ul>
                    </div>,
                    document.getElementById('app')
    )
~~~

### 组件定义

注意组件首字母需要大写

~~~javascript
const Mycomponent = (props) => {
    // props是一个对象，接收组件使用的时候传递过来的所有数据
	return jsx表达式
}
// 使用组件，传递数据arr,data
<MyComponent arr="" data={} />
~~~

##### 无状态组件

组件中无法定义state，只能接收props

~~~react
function ShowList(props){
    // props是一个对象，接收所有来自组件上传递的数据
      return <ul>
          {
            props.data.map(item =>{
              return <li key={item}>{item}</li>
            })
          }
        </ul>
    }
ReactDOM.render(<div>
        <ShowList data={arr}/>
      </div>,
      document.getElementById("app"))
~~~

##### 类组件

~~~react
class MyApp extends React.component {
    constructor (props) {
        super(props)
        this.state={}
    }
    render(){
        // 类组件中通过this.props获取传递过来的数据
        return <div>{this.props.data}</div>
    }
}
ReactDOM.render(<MyApp data={1} />,document.getElementById('app'))
~~~

#### 组件的属性

##### props

是一个对象，接收来自父组件传递的数据，组件内不予许修改props的值

* 在类组件中通过this.props.xxx来获取。
* 在无状态组件中直接通过形参props获取
* 类组件的constructor中super(props)作用是：在constructor中才能使用this.props获取父组件传递的数据

##### state  

在类组件的constructor中直接定义state

* 不要在其他钩子函数或者方法中直接修改state的值
* 要修改state的值用setState，它会触发渲染
* 直接修改state不会触发render，视图无法更新

多个this.setaState操作同一个属性时后面覆盖前面，因为它是一个异步操作。

方法支持一个回调，在回调中才能获得最新数据

  ~~~react
  constructor(props){
      super(props) //调用父类的构造函数
      this.state={
  		flag:false
  	}
  	// this.toggle=this.toggle.bind(this)
  }
  // 这里使用箭头函数才能找到当前实例，直接写函数的话，需要在constructor中绑定this
  //toggle () {
	//console.log(this.state)
  //}
  toggle = () => {
      
  // setState对象的语法，多个时后面覆盖前面
	this.setState({
        flag: !this.state.flag
    },()=>{
        // 可以获取到最新数据
        console.log(this.state.flag)
    })
    // 获取不到最新更改数据
    console.log(this.state.flag)
      
    // setState回调函数的语法，参数一为自己的数据，参数二是外部传递的数据，多个都会执行
    this.setState((prevState, props) => {
        retrun {
            n: preState.n + 1
        }
    })
}
  ~~~

##### refs  标识节点

~~~
//定义
ref="xxx"  使用this.refs.xxx获取
ref={(node)=>this.属性=node}  使用this.xxx获取
~~~

节点的获取在componentDidMount(组件挂载完成) 钩子函数下才能获取，通过this.refs.xxx

~~~react
class List extends React.Component {
	       	   componentDidMount(){
	       	   		//this.refs.txt.focus();
	       	       this.txt.focus();
	       	   }
	       	    render(){
	       	    	return  (
	       	    			<div>
	       	    				<!--<input type="text" ref="txt" />-->
	       	    				 <input type="text" ref={(node)=>this.txt=node} />
	       	    			</div>
	       	    			)
	       	    }
	       }
~~~

#### 受控/非受控组件

表单组件的值来自于state就是受控组件，否则为非受控组件（数据变化就能实现界面变化）

* 如果input的value或者checked绑定state的值，并且没有绑定任何onChange的方法来改变该绑定的state值时，输入框是无法输入内容的，或者复选框无法进行选择。要使用defaultValue，defaultChecked

### 钩子函数

#### 挂载期

组件实例被创建并插入DOM时，生命周期顺序

##### constructor(props)

* 如果不进行初始化state或者不进行方法绑定时，则不需要为组件实现构造函数
* 不要在构造函数内调用setState方法，直接为state赋值
* 先调用super(props)，否则可能导致this.props在构造函数中出现未定义的情况

##### static getDerivedStateFromProps()

 static 表示静态方法，它不能使用this调用

* 派生属性derivedState：state的属性值依赖于props

* 作用：获得最新派生属性的值

  父组件传递过来的props数据改变时，使用这个钩子才可以更新子组件的派生state

~~~react
static getDerivedStateFromProps(nextProps, prevState) {
    const {type} = nextProps;
    // 当传入的type发生变化的时候，更新state
    if (type !== prevState.type) {
        return {
            type,
        };
    }
    // 否则，对于state不进行任何操作
    return null;
}
~~~



##### render ()

组件定义必不可少，必须有返回值，返回jsx格式的内容

* return后面如果换行的话，使用括号可以将内容转为表达式

~~~react
render () {
    return <div>
      <input type="checkbox"  defauleChecked={this.state.check} />
      <button onClick={this.change}>点击</button>
    </div>
  }
~~~

##### componentDidMount   

节点挂载结束，可以进行Dom操作。

* 常做数据请求

* 可以使用this.refs.xxx获取节点进行操作  xxx是ref标识的名

* 此钩子中有this，调用其他组件的方法可以不用绑定this。

#### 更新期

组件的props或者state发生变化时触发

##### getDerivedStateFromProps(nextProps)

##### shouldComponentUpdate()

在数据更新时调用，即变化的才render，没有变化的不render，可以用作性能优化。

* 返回true才进行render，当存在列表渲染时，利用这个钩子可只渲染数据存在差别的，而不是全部渲染

* `PureComponent` 可取代此类性能优化(定义组件的用PureComponentComponent)、会对 props 和 state 进行浅层比较，并减少了跳过必要更新的可能性。

##### render()

##### getSnapshotBeforeUpdate(prevProps,prevState)

返回一个snapshot给componentDidUpdate，可以通过它获得dom状态

* 必须存在返回值
* 必须配合componentDidUpdate一起使用
* 在这里不能使用setState()，会导致死循环

##### componentDidUpdate(prevProps,prevState,snapshot)

起到监控数据的变化的作用，作用类似于vue中watch。当数据更新后执行，常应用与在componentDidmount中请求得到数据后，在这个钩子中进行渲染数据。

#### 卸载

当组件从DOM中移除时调用

##### componentWillUnmount

用于清理释放资源(清除定时器)，防止内存泄漏。

#### 被废弃钩子

* componentWillMount在挂载期间的render后面执行
* componentWillReceiveProps 
* componentBeforeUpdate 更新阶段的render后执行

### react性能优化

### 脚手架工具

#### 使用过程

安装`npm i create-react-app -g`

查看版本号'create-react-app --version'

新建项目`create-react-app  xxx`创建项目名为xxx的项目

配置脚手架环境位置：node_modules\react-scripts\目录下

运行项目：npm start

打包：npm run build

弹射： npm  run eject  弹射失败之后删除node_modl

#### 引入图片

* 先引入再使用

  引入：import 变量名 from '图片路径'  

  使用：\<img src={变量} />

* 使用时require

  <img src={import('图片路径')}

#### 使用sass

手动安装node-sass：`npm install node-sass -S`

## 数据相关

### 获取数据方式

#### fetch

请求返回一个包含响应结果的promise，是一个HTTP响应(response对象)，使用json()方法转换成json

~~~javascript
// 参数二可选，当需要配置请求方式，请求头，携带参数时进行配置
fetch('http://example.com/answer', {
    body: JSON.stringify(data), 
    headers: {
      'user-agent': 'Mozilla/4.0 MDN Example',
      'content-type': 'application/json'
    },
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
  })
  .then(resp => resp.json())
  .then(resp=>{
    console.log(resp)
	}
 )
~~~

#### axios

~~~javascript
axios.get('/user?ID=12345',{
	//携带参数
    params: {
      ID: 12345
    },
    //设置请求头
    headers: {}
})
  .then(function (response) {
    // 处理成功
    console.log(response);
  })
  .catch(function (error) {
    // 处理错误
    console.log(error);
  })
  .finally(function () {
    // 成功或错误都会执行
  });
~~~

##### baseURL处理方式

* 发请求的时候配置baseURL

* 在拦截器里面进行对请求的地址修改
* 定义一个变量

##### 正向/反向代理 

* 正向代理：由客户端将代理设置好目标服务器，向自己服务器发送请求后，由它代替自己去访问目标服务器

​	react中手动在config/jest/web....下面设proxy的目标服务器

~~~javascript
proxy: {
   "/mz/data": {
       // 假设通过代理到node提供的数据接口上
        "target": "https://localhost:3000",
        "changeOrigin": true,
        "pathRewrite": {
          "^/mz": "/"
        }
   }
}
~~~

* 反向代理： 客户端无需设置代理，前端访问，由后端到目标源请求数据后返回给前端，前端不会感知到代理的存在

### 数据提供

均存在跨域，需要进行正向代理

#### node提供

在node路由配置下，处理通过正向代理来自的数据接口的请求，如'/data'，并且将数据返回去.

~~~javascript
 // 处理来自/data的请求，通过resp.json将数据返回给请求端
router.get('/data',(req,resp) => {
  request.get("http://www.xiongmaoyouxuan.com/api/search/home",(err,response,body) => {
    var obj = JSON.parse(body)
    resp.json({
      list: obj.data.hotWords
    })
  })
})
~~~

#### json-server

安装：npm install json-server -g

配置默认端口（默认3000，和项目的默认端口存在冲突）：json-server xxx.json --port 4000

* 提供的数据只能供自己使用

* 配合mock假数据生成，再将随机数据返回抛到数据接口上

  ~~~javascript
  var Mock = require('mockjs')
  var fs = require('fs')
  // mock生成假数据
  var data = Mock.mock({
    'list|50':[{
      'id|+1':1,
      'name':'@cname'
    }]
  })
  //将数据写到指定文件
  fs.writeFileSync('E:/jsonData/mockdata.json',JSON.stringify(data),'utf-8')
  
  // 在E:/jsonData/  文件夹下执行json-server mockdata.json --port 4000
  // 之后就可以通过接口http:localhost:4000/xxx 请求到数据
  ~~~

### 接口文档生成

#### apidoc

全局安装`npm install apidoc -g`

使用：

* 在接口文件内按照官网要求格式写注释

  ~~~
  /**
   * @api {get} /user/:id Request User information
   * @apiName GetUser
   * @apiGroup User
   *
   * @apiParam {Number} id Users unique ID.
   *
   * @apiSuccess {String} firstname Firstname of the User.
   * @apiSuccess {String} lastname  Lastname of the User.
   */
  ~~~

* 在该接口文件所在目录下执行命令：`apidoc -i . -o doc/  `  意思是在当前目录(-o)下的doc文件夹下生成接口文档，随即生成接口文档。接口文档包含url，参数，请求方式，返回等

