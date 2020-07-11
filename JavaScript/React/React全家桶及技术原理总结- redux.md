# Redux

### 1. 什么是redux

​	Redux是⼀个⽤来管理理数据状态的JavaScript应⽤工具，它保证程序⾏为一

致性且易于测试。

 <img src="/Users/andcool/Documents/workspace/Javascript-Note/JavaScript/React/images/image-20200708210559367.png" alt="image-20200708210559367" style="zoom: 67%;" />

从图中可以看出，如果不用Redux，我们要传递state是⾮常麻烦的。 Redux中，可以把数据先放在数据仓库(store-公⽤用状态存储空间)中，这里可以统⼀管理状态，然后哪个组件用到了，就去stroe中查找状态。如果途中的紫⾊组件想改变状态时，只需要改变 store 中的状态，然后其他组件就会跟着中的自动进⾏改变。

### 2. Redux工作流

<img src="/Users/andcool/Documents/workspace/Javascript-Note/JavaScript/React/images/image-20200708210807817.png" alt="image-20200708210807817" style="zoom: 67%;" />

redux例子：累加计数器

>1. 需要⼀个store来存储数据
>2. store⾥的reducer初始化state并定义state修改规则
>3. 通过dispatch一个action来提交对数据的修改
>4. action提交到reducer函数⾥，根据传⼊的action的type，返回新的state

**1.创建store**

```js
import {createStore} from 'redux'
const store = createStore() 
export default store
```
**2.创建reducer，帮助store管理更新状态**

```js
import { createStore } from "redux";
const counterReducer = (state = 0, action) => { 
	return state;
};
const store = createStore(counterReducer);
export default store;
```

**3. getState获取store状态**

```js
 
import React, { Component } from "react";
import store from "../store";
export default class Counter extends Component {
  render() {
    return (
      <div>
				<p>{store.getState()}</p>
			</div>
		);
	}
}
```

**4.派发dispatch,通过action type描述具体⾏为**

```js
import React, { Component } from "react"; 
import store from "../store";
export default class Counter extends Component {
  render() {
    return (
      <div>
				<p>{store.getState()}</p>
				<button onClick={() => store.dispatch({ type: "add" })}>+ </button>
				<button onClick={() => store.dispatch({ type: "minus" })}>- </button>
			</div>
		);
	}
}
```

**5.reducer接收action，通过type指定⾏为**

````js
switch (action.type) {
	case "add":
		return state + 1; 
	case "minus":
		return state - 1; 
	default:
    return state;
}
````

> 点击按钮不能更新，因为没有订阅状态变更

**6.订阅状态变更**

```js
1.=====================根组件===========================
import store from './store'
const render = ()=>{
	ReactDom.render( 
    <App/>,
		document.querySelector('#root') 
  )
} 
render()
store.subscribe(render)

2.==================组件内========================= 
componentDidMount() {
	store.subscribe(() => this.forceUpdate()); 
}
```

>**注意点**
>
>1. createStore创建store
>2. reducer初始化、修改状态函数 
>3.  getState获取状态值
>4. dispatch提交更新
>5. subscribe变更订阅

### 3. redux工作流梳理

- redux是⼀个存储状态，响应事件动作(action)的地⽅方，所以定义redux 实现的叫store
-  store有⼀个初始状态(default state)，还有响应某个动作(action)的处理器(reducer)
- 然后UI视图将这个store及其状态(state)和方法(action)注册到视图组件的props，这样就可以在组件中取到这些状态和方法了。 
- 当⽤户点击了某个操作等，会从props中拿到action并调⽤用它，他会向 store发送(dispatch)这个action的内容，
- 如果store中有中间件，会先逐个调用中间件来完成预处理
- 然后再调⽤各个reducer，来完成状态的改变。
- 状态改变以后，因为状态绑定了UI组件的props，所以react会⾃动刷新 UI。

### 4.react-redux

每次都重新调用render和getState太low了，想⽤更react的方式来写，需 要react-redux的支持

Redux 官⽅提供的 React 绑定库。 具有⾼效且灵活的特性。 

简易流程图

<img src="/Users/andcool/Documents/workspace/Javascript-Note/JavaScript/React/images/image-20200708212150937.png" alt="image-20200708212150937" style="zoom:67%;" />

**react-redux提供两个api**

>1. Provider为后代组件提供store
>2. connect为组件提供数据和变更方法
>     ​mapStateToProps这个函数允许我们将 store 中的数据作为 props 绑定到组件上。
>     ​mapDispatchToProps将 dispatch作为 props 绑定到组件上

全局提供store，index.js

```js
import React from 'react'
import ReactDom from 'react-dom' 
import App from './App'
import store from './store'
import { Provider } from 'react-redux' 
ReactDom.render(
	<Provider store={store}> 
  	<App/>
  </Provider>,
	document.querySelector('#root') 
)
```

获取状态数据，ReduxTest.js

```js
// import store from "../store";
import { connect } from "react-redux";
@connect(
	state => ({ num: state }), // 状态映射 
  {
		add: () => ({ type: "add" }), // action creator
		minus: () => ({ type: "minus" }) // action creator 
  }
)
class Counter extends Component {
  render() {
    return (
			<div> 
      	<p>{this.props.num}</p>
     	 <div>
				<button onClick={this.props.add}>+</button>
				<button onClick={this.props.minus}>-</button>
			 </div>
			</div> 
		);
	} 
}

function mapStateToProps(state) {
  return {
		num: state 
  };
}

function mapDispatchToProps(dispatch) {
	return {
		add: () => dispatch({ type: "add" }), 
    minus: () => dispatch({ type: "minus" })
	};
}

const App = connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter);

export default App;
```

>connect中的参数:
> const mapStateToProps = (state) => { 
>​	return {
>		 num: state 
>	}
>}
>const mapDispatchToProps = dispatch => {
> 	return {
> 		add: () => dispatch({type:"add"}),
> 		minus: () => dispatch({type:"add"})>
>	}
>}

### 5. 异步

react默认只支持同步，实现异步任务比如延迟，⽹络请求，需要中间件的支持，⽐如我们试用最简单的redux-thunk和redux-logger

**应用中间件， store.js**

```js
import { createStore, applyMiddleware } from "redux"; 
import logger from "redux-logger";
import thunk from "redux-thunk";
const store = createStore(fruitReducer, applyMiddleware(logger, thunk));
```

使⽤异步操作时的变化，ReduxTest.js

```js
@connect(
	state => ({ num: state }), 
  {
    ...,
    asyncAdd: () => dispatch => {
      setTimeout(() => {
        // 异步结束后，⼿动执行dispatch 
        dispatch({ type: "add" });
      }, 1000);
    }
  }
)
```

### 6. redux原理

**核心实现**

>存储状态state 
>获取状态getState 
>更新状态dispatch 
>变更订阅subscribe

````js
 
export function createStore(reducer, enhancer){
//增强createStore 
  if (enhancer) {
    return enhancer(createStore)(reducer)
  }
  // 保存状态
  let currentState = undefined;
  // 回调函数,将每一个订阅了store的组件listener存储
  let currentListeners = [];
  function getState(){
    return currentState
  }
  function subscribe(listener){
    currentListeners.push(listener) 
  }
  function dispatch(action){ 
    //更新状态
    currentState = reducer(currentState, action) 
    //变更通知
    currentListeners.forEach(v=>v())
    return action
  }
	//初始化状态 
	dispatch({type:'@IMOOC/KKB-REDUX'}) 
	return { getState, subscribe, dispatch}
}
````

**测试代码，components/MyReduxTest.js**

```js
import React, { Component } from "react";
import { createStore } from "../store/redux";
const counterReducer = (state = 0, action) => {
  switch (action.type) { 
    case "add":
      return state + 1; 
    case "minus":
      return state - 1; 
    default:
      return state;
  }
};
const store = createStore(counterReducer);
export default class MyReduxTest extends Component {
	componentDidMount() {
    store.subscribe(() => this.forceUpdate());
  }
  render() {
    return (
      <div>
        {store.getState()}
        <button onClick={() => store.dispatch({ type: "add"})}>+</button>
      </div>
		);
	}
}
```

### 7. 中间件的实现

核心任务是实现函数序列执⾏

<img src="/Users/andcool/Documents/workspace/Javascript-Note/JavaScript/React/images/image-20200708214252673.png" alt="image-20200708214252673" style="zoom:67%;" />

函数聚合

**[fn1,fn2,fn3]**按照从左到右执⾏ 聚合成⼀个函数**Fn3(fn2(fn1()))**

```js
export function applyMiddleware(...middlewares){ // 返回强化以后函数
	return createStore => (...args) => { //完成createStore该有的⼯工作
		const store = createStore(...args) //原先的dispatch
		let dispatch = store.dispatch //传递给中间件的参数
		const midApi = {
      getState:store.getState,
      dispatch:(...args)=>dispatch(...args)
    }
		// 使中间件可以获取状态值、
		const middlewareChain = middlewares.map(middleware => middleware(midApi))
		// compose可以middlewareChain函数数组合并成⼀个函数 
    //强化后的dispatch，按顺序执⾏行行中间件函数
		dispatch = compose(...middlewareChain)(store.dispatch) 
    return {
    	//返回全新没有修改过的store,和增强过的dispatch 
      ...store,
   		dispatch
    } 
  }
}
export function compose(...funcs){
	if (funcs.length==0) {
    return arg=>arg
	}
	if (funcs.length==1) {
    return funcs[0]
  }
	return funcs.reduce((left,right) => (...args) => right(left(...args)))
}
```

**测试代码，MyReduxTest.js**

```js
import { applyMiddleware } from "../store/redux";
function logger({dispatch, getState}) { 
  	return dispatch => action => {
      // 中间件任务
      console.log(action.type + '执⾏行行了了!!'); // 下⼀一个中间件
      return dispatch(action);
		}
}
const store = createStore(counterReducer, applyMiddleware(logger));
```



### API

#### 1. combineReducers(reducers)

随着应⽤变得越来越复杂，可以考虑将 reducer 函数 拆分成多个单独的函 数，拆分后的每个函数负责独⽴管理 state 的一部分。

> 函数原型: combineReducers(reducers)

- 参数:reducers (Object): 一个对象，它的值(value)对应不同的 reducer 函数，这些 reducer 函数后⾯面会被合并成⼀个。下⾯会介绍传 入 reducer 函数需要满足的规则。

- 每个传入 combineReducers 的 reducer 都需满⾜足以下规则:
     - 所有未匹配到的 action，必须把它接收到的第⼀个参数也就是那个 state 原封不动返回。
     - 永远不能返回 undefined。当过早 return 时⾮常容易犯这个错 误，为了避免错误扩散，遇到这种情况时 combineReducers 会抛 异常。
     - 如果传⼊的 state 就是 undefined，⼀定要返回对应 reducer 的初始 state。根据上一条规则，初始 state 禁止使用undefined。使⽤用 ES6 的默认参数值语法来设置初始 state 很容易，但你也可以手动检查第一个参数是否为 undefined。

返回值

(Function):⼀个调用 reducers 对象里所有 reducer 的 reducer，并且构造一个与 reducers 对象结构相同的 state 对象。

combineReducers 辅助函数的作⽤是，把⼀个由多个不同 reducer 函数作 为 value 的 object，合并成⼀个最终的 reducer 函数，然后就可以对这个 reducer 调用 createStore ⽅法。

合并后的 reducer 可以调⽤各个子 reducer，并把它们返回的结果合并成 ⼀个 state 对象。 由 combineReducers() 返回的 state 对象，会将传入的每个 reducer 返回的 state 按其传递给combineReducers() 时对应的 key 进行命名。

>提示:在 reducer 层级的任何⼀级都可以调用 combineReducers。并不是一定要在最外层。实际上，你可以把一些复杂的子 reducer 拆分成单独的孙子级 reducer，甚⾄更多层。

#### 2. createStore

> 函数原型:createStore(reducer, [preloadedState], enhancer)

参数

- reducer (Function)::项⽬目的根reducer。

- [preloadedState] (any) :这个参数是可选的, ⽤于设置 state 初始状态。这对开发同构应⽤时非常有⽤用，服务器端 redux 应⽤的 state 结构可以与客户端保持一致, 那么客户端可以将从⽹络接收到的服务端 state 直接⽤于本地数据初始化。

- enhancer (Function) : Store enhancer 是一个组合 store creator 的高阶函数，返回一个新的强化过的 store creator。这与middleware相似，它也允许你通过复合函数改变 store 接口。

返回值

- (Store) : 保存了应用所有 state 的对象。改变 state 的唯一⽅法是 dispatch action。你也可以 subscribe 监听 state 的变化，然后更新 UI。

示例

```js
 import { createStore } from 'redux'
function todos(state = [], action) { 
  switch (action.type) {
		case 'ADD_TODO':
			return state.concat([action.text])
    default:
      return state
	}
}
let store = createStore(todos, ['Use Redux'])
store.dispatch({
	type: 'ADD_TODO', 
  text: 'Read the docs'
})
console.log(store.getState())
// [ 'Use Redux', 'Read the docs' ]
```

注意事项

- 应用中不要创建多个 store! 相反，使⽤ combineReducers 来把多个 reducer 创建成一个根 reducer。
- 你可以决定 state 的格式。你可以使用普通对象或者 Immutable 这类 的实现。如果你不知道如何做，刚开始可以使用普通对象。
- 如果 state 是普通对象，永远不要修改它!比如，reducer ⾥不要使用 Object.assign(state, newData)，应该使⽤ Object.assign({}, state,newData)。这样才不会覆盖旧的 state。如果可以的话，也可以使⽤对象拓拓展操作符(object spread spread operator 特性中的 return { ...state, ...newData }。
- 对于服务端运⾏的同构应用，为每⼀个请求创建⼀个 store 实例，以此 让 store 相隔离。dispatch ⼀系列请求数据的 action 到 store 实例上，等待请求完成后再在服务端渲染应用。
- 当 store 创建后，Redux 会 dispatch ⼀个 action 到 reducer 上，来⽤初始的 state 来填充 store。你不需要处理这个 action。但要记住，如 果第一个参数也就是传入的 state 是 undefined 的话，reducer 应该返回初始的 state 值。
- 要使用多个 store 增强器的时候，你可能需要使用 compose

#### 3. applyMiddleware

> 函数原型: applyMiddleware(...middleware)

使用包含⾃定义功能的 middleware 来扩展 Redux。

#### 4.如何做到从不直接修改 **state** ?

从不直接修改 state 是 Redux 的核⼼心理理念之一:为实现这一理念，可以通过一下两种方式

> 1. 通过**Object.assign()**创建对象拷⻉贝, ⽽拷贝中会包含新创建或更新过的属性值
>
>    在下⾯面的 todoApp 示例中, Object.assign() 将会返回一个新的 state 对象, ⽽其中的 visibilityFilter 属性被更新了:
>
>    ```js
>    function todoApp(state = initialState, action) { 
>      switch (action.type) {
>    		case SET_VISIBILITY_FILTER:
>    			return Object.assign({}, state, { visibilityFilter: action.filter}) 			default:
>          return state
>      }
>    }
>    ```
>
> 2. 通过**ES7**的新特性**[**对象展开运算符**(Object Spread Operator)]**
>
>    ```js
>    function todoApp(state = initialState, action) { 
>      switch (action.type) {
>    		case SET_VISIBILITY_FILTER:
>    			return { ...state, visibilityFilter: action.filter }
>      	default:
>        	return state
>    	} 
>    }
>    ```
>
>    这样你就能轻松的跳回到这个对象之前的某个状态(想象一个撤销功 能)。

## 总结

- Redux 应⽤只有一个单⼀的store。当需要拆分数据处理逻辑时，你应该使用reducer 组合，而不不是创建多个 store; 
- redux⼀个特点是:状态共享，所有的状态都放在一个store中，任何 component都可以订阅store中的数据;
- 并不是所有的state都适合放在store中，这样会让store变得非常庞大， 如某个状态只被一个组件使⽤，不存在状态共享，可以不放在store 中;