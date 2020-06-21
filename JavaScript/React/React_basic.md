#### 1、Html中使用react

通过CDN链接获得React和ReactDOM的UMD版本号

开发环境，不适合生产环境

```html
 <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"> </script>
<script crossorigin src="https://unpkg.com/react- dom@16/umd/react-dom.development.js"></script>
```

生产环境

```html
 <script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js "></script>
<script crossorigin src="https://unpkg.com/react- dom@16/umd/react-dom.production.min.js"></script>
```

​	如果加载指定版本的react和react-doe，可以替换src中指定的版本号。

如果通过CDN的方式引入React，建议设置crossorigin属性

---

#### 2、CRA工具的使用

Create React App是一个官方支持的创建React单页面应用程序方法。它提供了零配置的现代化构建设置。

安装官⽅方脚⼿手架:

```bash
npm install -g create-react-app
```

创建项目：

```bash
create-react-app my-app
```

启动项目：

```bash
npm start / yarn start
```

构建生产：

```bash
npm run build / yarn build
```

webpack扩展：

```bash
npm run eject
```
---
#### 3、 JSX

JSX是⼀一种JavaScript的语法扩展，其格式⽐比较像模版语⾔言，但事实上完全是在JavaScript内部实现的。JSX 仅仅只是**React.createElement(component, props, ...children)** 函数的语法糖。JSX可以很好地描述UI，能够有效提⾼高开发效率。

**JSX实质就是React.createElement的调⽤用，最终的结果是React“元 素”(JavaScript对象),React"元素"类型可以是原⽣生Dom，字⺟母⼩小写， ⾸首字⺟母⼤大写的视为⾃自定义组件。**

```js
const jsx = <h2>hello React</h2>;
ReactDOM.render(jsx, document.getElementById('root'));
```

在jsx中通过{}使用表达式

```js
const title = "hello React";
const jsx = <h2>{title}</h2>;
```

使用函数表达式

```js
const user = { firstName: "tom", lastName: "jerry" };
function formatName(user) {
	return user.firstName + " " + user.lastName; 
}
const jsx = <h2>{formatName(user)}</h2>;
```

React"元素"也是合法表达式

```js
const subTitle = <p>hello, React</p>
const jsx = <h2>{subTitle}</h2>;
```

---

#### 4、动态渲染UI

```js
 
function tick() { 
	const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
	); 
	ReactDOM.render( element, document.getElementById('root') );
}
setInterval(tick, 1000);
```

---

#### 5、条件渲染

if 语句以及 for 循环不是 JavaScript 表达式，所以不不能在 JSX 中直接使用。但是，你可以用在 JSX 以外的代码中。可以基于上⾯面结论实现。

**if语句**

```js
let isShowTitle = true; 
if (isShowTitle) {
	title = <h1>是否显示h1</h1>; 
}
```

**逻辑与&&**

```js
 
const jsx = ( <div>
    {isShowTitle && <h1>title1</h1>}
  </div>
);
ReactDOM.render(jsx, document.querySelector("#root"));
```

**三元表达式||三⽬目运算符**

```js
const showTitle = true;
const title = name ? <h2>{name}</h2> : null; const jsx = (
<div>
{/* 条件语句句 */}
    {title}
  </div>
);
```

**循环列列表&& key**

数组会被作为⼀组子元素对待，数组中存放⼀组jsx可用于显示列表数据

```js
 const arr = [1,2,3].map(num => <li key={num}>{num}</li>) 
 const jsx = (
  <div>
  	{/* 数组 */}
      <ul>{arr}</ul>
  </div>
);
```

**React Dom元素属性的使用**

```js
import logo from "./logo.svg"; import "index.css";
const box = { color:"blue", border:"1px blue solid"}
const jsx = (
<div style={box}>
  {/* 属性:静态值⽤用双引号，动态值⽤用花括号;class、for等要特殊处
  理理。 */}
  <img 
    src={logo} 
    style={{ width:100,height:100,border:"1px red solid" }} 
    className="width height" 
  />
</div>
);
```

css模块化，创建index.module.css，index.js

```js
import style from "./index.module.css";
<img className={style.img} />
<img className={`${style.font14} ${style.red}`} />
```

**样式解决方案**

- [classnames](https://github.com/JedWatson/classnames)
- [styled-components](https://github.com/styled-components/styled-components)
- [styled-jsx](https://github.com/zeit/styled-jsx)

---

#### 6、组件

组件允许你将 UI 拆分为独立可复⽤的代码⽚段，并对每个⽚段进行独立构思。

组件类型 

- 函数组件

- class组件

##### 函数组件

定义组件最简单的方式就是函数组件，本质上就是JavaScript函数

函数组件通常无状态，仅关注内容展示，返回渲染结果即可 (从React16.8开始引⼊入了了**hooks**，函数组件也能够拥有状态，后面组件状态管理部分讨论)

```js
  function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
  }
```
##### 组件props

当 React 元素为⽤户⾃定义组件时，它会将 JSX 所接收的属性

(attributes)转换为单个对象传递给组件，这个对象被称之为 “props”。

##### 组件使用

```js
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>;
}
const element = <Welcome name="Sara" msg={'消息'} arr= {[1,2,3,4]} obj={{id:0}}/>;
ReactDOM.render(
	element,
	document.getElementById('root') 
);
```

##### 渲染过程分析

1. 我们调⽤用 ReactDOM.render() 函数，并传⼊入 <Welcome name="Sara" /> 作为参数。
2. React 调⽤用 Welcome 组件，并将 {name: 'Sara'} 作为 props 传⼊入。 
3. **Welcome** 组件将 <h1>Hello, Sara</h1> 元素作为返回值。
4. ReactDOM将DOM⾼效地更新为<h1>Hello,Sara</h1>。

##### 组件注意事项

组件名称必须以⼤写字母开头。

React 会将以小写字⺟开头的组件视为原生 DOM 标签。例例如，<div /> 代表 HTML 的 div 标签，而 <Welcome /> 则代表一个组件

return的内容只能有一个根节点，需要一个包裹元素，⽐如使用 ，如果想返回多个兄弟元素，不想额外的嵌套，可以使用数组的方式或者**Fragments**。
所有 **React** 组件都必须像纯函数⼀样保护它们的 **props** 不被更改。

```
 
-----------数组⽅方式-------------- 
return [<h1/>,<h2/>,<h3/>]
-----------Fragments------------ 
return (
  <React.Fragment> <h1 />
  <h2 />
  <h3 /> </React.Fragment>
);
//短语法 
return (
  <>
    <h1 />
    <h2 />
    <h3 />
	</> 
);
注意:使⽤用显式 `<React.Fragment>` 语法声明的⽚片段可能具有 key。 
<dl>
  {props.items.map(item => (
    // 没有`key`，React 会发出⼀个关键警告 
    <React.Fragment key={item.id}>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </React.Fragment>
  ))}
</dl>
```

##### class组件

使⽤ES6 的 class 来定义组件，称为class组件

class组件通常拥有状态和⽣命周期，继承于**Component**，实现**render**⽅法

```js
import React from "react";
class Welcome extends React.Component { 
  //通过构造函数接受props,可以省略略
  render() {
  return <h1>Hello, {this.props.name}</h1>;
      //return null
  } 
}
```

render ⽅方法直接返回 null，表示不进⾏任何渲染。

##### 组件状态管理

如果组件中数据会变化，并影响页⾯内容，则组件需要拥有状态(state) 并维护状态。

props 是在⽗组件中指定，而且⼀经指定，不再改变。 对于需要改变的数 据，我们需要使用 state 。

state状态改变，组件会重新调用render⽅法，更新UI

###### 类组件中的状态管理理

class组件通过state和setState维护状态 创建⼀个Clock组件，改造之前的tick案例

```js
 
class Clock extends React.Component { 
  constructor(props) {
		super(props);
		// 使⽤用state属性维护状态，在构造函数中初始化状态 
    this.state = { date: new Date() };
  }
  componentDidMount() {
    // 组件挂载时启动定时器器每秒更更新状态 
    this.timerID = setInterval(() => {
    	// 使⽤用setState⽅方法更更新状态 this.setState({
        date: new Date()
      });
		}, 1000); 
	}
 
  componentWillUnmount() {
  	// 组件卸载时停⽌止定时器器 
    clearInterval(this.timerID);
  }
  render() {
  	return <div>{this.state.date.toLocaleTimeString()} </div>;
  } 
}
```

拓展:**setState**特性讨论

- 不要直接修改state，要用setState更新状态

  ```js
  this.state.counter += 1; //错误的
  ```

- state的更新会被合并:当你调用 setState() 的时候，React 会

  把你提供的对象合并到当前的 state，这里的合并是浅合并。

- state的更更新可能是异步的

  对于多个setState执⾏行行，会合并成⼀一个调⽤用，因此对同⼀一个状 态执⾏行行多次只起⼀一次作⽤用，多个状态更更新合并在⼀一个setState 中进行：

  ```js
  componentDidMount() {
  // 假如couter初始值为0，执⾏行行三次以后其结果是多少? 
    this.setState({counter: this.state.counter + 1}); 
    this.setState({counter: this.state.counter + 1}); 
    this.setState({counter: this.state.counter + 1}); 
  }
  ```

  获取到最新状态值有以下三种⽅式:

  1、传递函数给setState⽅法:可以让 setState() 接收一个函数而不是一个对象。这个函数用上一个 state 作为第⼀个参数，将此次更新被应用时的 props 做为第⼆个参数

  ```js
   
  this.setState((state, props) => ({ counter: 
                                    state.counter + 1}));// 1 
  this.setState((state, props) => ({ counter: 
                                    state.counter + 1}));// 2 
  this.setState((state, props) => ({ counter: 
                                    state.counter + 1}));// 3
  //es5
  this.setState(function(state, props) { 
    return {
  		counter: state.counter + 1 
    };
  });
  ```

  2、使用定时器

  ```js
  setTimeout(() => { 
  	console.log(this.state.counter);
  }, 0);
  ```

  3、原⽣生事件中修改状态

  ```js
  componentDidMount(){ 
    document.body.addEventListener('click', this.changeValue, false) 
  }
  changeValue = () => {
    this.setState({counter: this.state.counter+1}) 
    console.log(this.state.counter)
  }
  ```

#### 事件处理

React 事件的命名采用小驼峰式(camelCase)，⽽不是纯⼩写
使用 JSX 语法时你需要传⼊一个函数作为事件处理函数，⽽不是⼀个字符串

例如:onClick={activateLasers}

范例:用户输入事件，创建EventHandle.js

```js
import React, { Component } from "react";
export default class EventHandle extends Component {
  constructor(props) {
    super(props);
		this.state = { name: ""};
    this.handleChange = this.handleChange.bind(this);
  }
  handleChange(e) {
  	this.setState({ name: e.target.value });
  }
  render() {
    return (
      <div>
        {/* 使⽤用箭头函数，不不需要指定回调函数this，且便便于传递参数 */}
        {/* <input
              type="text"
              value={this.state.name}
              onChange={e => this.handleChange(e)}
            /> 
        */}
        {/* 直接指定回调函数，需要指定其this指向，或者将回调设置为 箭头函数属性 */}
      <input
        type="text" 
        value={this.state.name} 
        onChange={this.handleChange(e)}
      />
      <p>{this.state.name}</p>
      </div>
    ); 
	}
}
```

>事件回调函数注意绑定this指向，常⻅见三种⽅方法:
>
>1. 构造函数中绑定并覆盖:this.textChange= this.textChange.bind(this)
>
>2. ⽅法定义为箭头函数:textChange=()=>{}
>
>3. 事件中定义为箭头函数:onChange={()=>this.textChange()}
>
>   react⾥里遵循单项数据流，没有双向绑定，输入框要设置value和 onChange，称为受控组件

#### 组件通信

Props属性传递 遵守单项数据流

Props属性传递可⽤于⽗子组件相互通信

如果⽗组件传递的是函数，则可以把⼦组件信息传⼊父组件，这个通常称为状态提升，StateMgt.js

```js
// StateMgt
<Clock change={this.onChange}/>

// Clock
this.timerID = setInterval(() => { 
  this.setState({
  	date: new Date() 
  }, ()=>{
  // 每次状态更更新就通知⽗父组件
  	this.props.change(this.state.date); 
  });
}, 1000);
```

##### context

跨层级组件之间通信

##### redux

类似vuex，⽆明显关系的组件间通信