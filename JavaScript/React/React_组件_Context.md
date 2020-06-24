### 组件跨层级通信 - Context

Context提供了了⼀个无需为每层组件手动添加props，就能在组件树间进⾏

数据传递的⽅法。 React中使⽤Context实现祖代组件向后代组件跨层级传值。Vue中的

provide & inject来源于Context 

在Context模式下有两个⻆色:

- Provider:外层提供数据的组件
- Consumer :内层获取数据的组件

#### 1、使用Context

1. 创建Context
2. 获取Provider和Consumer 
3. Provider提供值
4. Consumer消费值

```js
import React from "react"; // 创建上下⽂文
const Context = React.createContext();
// 获取Provider和Consumer
const Provider = Context.Provider;
const Consumer = Context.Consumer;
// Child显示计数器器，并能修改它，多个Child之间需要共享数据 
function Child(props) {
  return <div onClick={() => props.add()}>{props.counter} </div>;
}
export default class ContextTest extends React.Component { // state是要传递的数据
  state = {
    counter: 0
  };
  // add⽅方法可以修改状态 
  add = () => {
    this.setState(nextState => ({ counter: nextState.counter + 1 }));
  };
  // counter状态变更

  render() {
    return (
      <Provider value={{ counter: this.state.counter, add: this.add }}>
        {/* Consumer中内嵌函数，其参数是传递的数据，返回要渲染的组 件 */}
        {/* 把value展开传递给Child */}
        <Consumer>{value => <Child {...value} />} </Consumer>
        <Consumer>{value => <Child {...value} />} </Consumer>
      </Provider>
    );
  }
}
```

#### 2、扩展:Context API

- React.createContext(defaultValue)

  - 返回带有Provider组件 Consumer组件的对象

- Context.Provider

  - 组件是数据的发布方，⼀般在组件树的上层并接收⼀个数据的初始

  值;

- Class.contextType

  - 挂载在 class 上的 contextType 属性会被重赋值为⼀个由 React.createContext() 创建的 Context 对象。这能让你使用 this.context 来消费最近 Context 上的那个值。你可以在任何⽣命周期中访问到它，包括 render 函数中。

- Context.Consumer
  
  - 组件是数据的订阅方，它的 props.children 是⼀个函数，接收被发布的数据，并且返回 React Element
  - 能让你在函数式组件中完成订阅 context
  - 这个函数接收当前的 context 值，返回一个 React 节点。传递给函数的 value 值等同于往上组件树离这个 context 最近的 Provider 提供的 value 值。如果没有对应的 Provider，value 参数等同于传递给 createContext() 的 defaultValue。
  
```js
 
const Context = React.createContext("defValue");
//在组件中使⽤用contextType,获取Context的默认值 
class Child extends Component{
// static contextType = Context; 实验性的语法 
  render() {
  	return <div>{this.context}</div> 
  }
 }
Child.contextType = Context;
function App(){ //Child不不能为函数组件，因为contextType只能挂载到class组件 
  return <Child/>
}
```