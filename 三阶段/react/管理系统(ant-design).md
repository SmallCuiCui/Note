# 部分补充

## better-scroll

它是由原生js写的插件，用于处理滚动事件的，vue与react都可以使用，但是它只能用于移动端项目。

安装`cnpm install better-scroll`

使用：新建better-scroll实例，参数一是滚动元素，参数二是一个对象，配置一些参数，比如滚动方向

~~~
import React,{Component} from "react"
import BS from "better-scroll"

// componentDidMount(){
	this.scroll = new BS(this.refs.App,)
}
~~~

### 注意坑点

* 新建实例的时候，在参数二的字段中添加mouseWheel: true字段，当在pc端使用鼠标模拟滑动时才不会出问题
* 

## 百度地图PAI

## Hook

使用hook可以使无状态组件含有状态。

### useState

useState()方法只有一个参数，就是初始的state值。方法返回两个值，一个就是当前的state，一个是修改state的方法。该方法可多次调用，定义多个state

~~~react
import React,{useState} from "react"
function One(){
	const [count, setCount] = useState(0)
    const [obj, setObj] = useState({a:1,b:2})
    const [todos, setTodos] = useState([{ text: 'Learn Hooks' },{ text: 'Learn react' }]);
    return </div>
        <p>count</p>
    	<ul>
			{
				todos.map(item => {
					return <li key={item.text}>item.text</li>
				})
			}
		</ul>
    	<button onClick={()=>{setCount(count+1)}}>点击</button>
        </div>
}
~~~

### useEffect

此方法相当于组件的componentDidMount与ComponentDidUpdate。在组建创建以及DOM更改之后调用，它的参数是一个回调函数，在该回调中执行操作

~~~react
import React,{useState,useEffect} from "react"
function One(){
	const [count, setCount] = useState(0)
	}]);
	// 当组件创建以及数据发生变化，DOM改变之后执行
    useEffect(()=>{
    	console.log(1)
    })
	return <div>
		<p>{count}</p>
		<button onClick={()=>{setCount(count+1)}}>点击</button>
	</div>
}

export default One
~~~

# 管理系统

创建项目`create-react-app xx`

antd  实现按需加载  axios  redux,react-redux,redux-thunk  react-router-dom

## ant-design

是一个react的UI库（PC端）官网：ant.design

安装`cnpm install antd`

### 加载模式

#### 全部加载

css文件内引入.css文件，引入组件。此方法存在弊端，它加载了全部的css样式。

~~~react
@import '~antd/dist/antd.css';// css文件内引入
import Button from 'antd/es/button';
function App() {
  return (<Button>点击</Button>);
}
~~~

#### 按需加载*

##### 使用过程

1. 使用 react-app-rewired

   对 create-react-app 的默认配置package.json进行自定义配置

   * 安装： `cnpm install react-app-rewired customize-cra`

   * 修改package.json文件，修改scripts字段，将所有的react-scripts替换为react-app-rewired

     ~~~js
     "scripts": {
         "start": "react-app-rewired start",
         "build": "react-app-rewired build",
         "test": "react-app-rewired test",
         "eject": "react-app-rewired eject"
       }
     ~~~

   * 根目录下创建config-overrides.js文件，暂时不用配置，下一步再配置

2. 使用 babel-plugin-import

   该依赖的作用是实现按需加载组件与样式的babel插件

   * 安装`cnpm install babel-plugin-import`

   * 编辑config-overrides.js文件为：

     ~~~react
     const { override, fixBabelImports } = require('customize-cra');
     
     module.exports = override(
        fixBabelImports('import', {
          libraryName: 'antd',
          libraryDirectory: 'es',
          style: 'css',
        }),
      );
     ~~~

3. 使用按需加载

   对比全部加载模式下，去掉app.css文件下全部引入的样式`@import '~antd/dist/antd.css`

   组件的使用去掉 import Button from 'antd/es/button';

   ~~~react
   // 直接加载需要的组件即可，样式也会随着一起加载
   import { Button } from 'antd';
   ~~~

##### 装饰器

* 安装依赖  `cnpm install @babel/plugin-proposal-decorators`
* config-overrides.js  中引入addDecoration，并且在override中使用addDecoration()方法即可
* 使用：替代hoc，connect，withRouter包裹组件，直接使用下面的方法替代；

~~~react
// 注释部分是未使用修饰器的写法
// export default hoc(One)
@hoc
export default One
// connect((state)=>state,actionCreator)(One)
@connect((state)=>state,actionCreator)
export default One
// export default withRouter(One)
@withRouter
export default One
~~~

### 定制主题

* 安装：`cnpm install less less-loader`注意安装的是两个依赖

* 根目录下新建theme.js文件，采用module.export导出

~~~
module.exports = {
	"@primary-color": "black", // 全局主色
	"@link-color": "#1890ff", // 链接色
	"@success-color": "#52c41a", // 成功色
	"@warning-color": "#faad14", // 警告色
	"@error-color": "#f5222d", // 错误色
	"@font-size-base": "14px", // 主字号
	"@heading-color": "rgba(0, 0, 0, 0.85)", // 标题色
	"@text-color": "rgba(0, 0, 0, 0.65)", // 主文本色
	"@text-color-secondary ": "rgba(0, 0, 0, .45)", // 次文本色
	"@disabled-color ": "rgba(0, 0, 0, .25)", // 失效色
	"@border-radius-base": "4px", // 组件/浮层圆角
	"@border-color-base": "#d9d9d9", // 边框色
	"@box-shadow-base": "0 2px 8px rgba(0, 0, 0, 0.15)", // 浮层阴影
}
~~~

* 配置config-overrides.js文件的override字段
  * 引入使用addLessLoader，引入theme.js并添加
  * 修改fixBabelImports的color为true

~~~react
const theme = require("./theme")
const { override, addLessLoader } = require('customize-cra');
module.exports = override(
	// 装饰器
   // 按需加载的插件
   fixBabelImports('import', {
     libraryName: 'antd',
     libraryDirectory: 'es',
     style: true
   }),
   // 自定义主题
   addLessLoader({
   javascriptEnabled: true,
   modifyVars: theme,
 })
 );

~~~

* 使用主题，对应所有ant-design的组件颜色的选择，比如选择type=primary就对应primary-color

  ~~~
  import { Button } from 'antd';
   <Button type="primary">Primary</Button>
  ~~~


### 常用组件

- spin 
- badge



## ant-design  mobile

