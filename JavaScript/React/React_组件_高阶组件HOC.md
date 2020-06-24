### 1、⾼阶组件 - HOC

高阶组件(HOC:Higher-Order Components)是 React 中⽤于复⽤组件逻辑的一种⾼级技巧。

HOC 自身不不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。

**⾼阶组件是一个⼯厂函数，参数为组件，返回值为新组件的函数**

为了提⾼组件复⽤用率，可测试性，就要保证组件功能单⼀性;但是若要满足复杂需求就要扩展功能单⼀的组件，在React⾥就有了了HOC的概念。

```js
// Hoc.js
import React from "react";
// Lesson保证功能单⼀一，它不不关⼼心数据来源，只负责显示 
function Lesson(props) {
  return (
    <div>
      {props.stage} - {props.title}
    </div>
  );
}
// 模拟数据
const lessons = [
  { stage: "React", title: "核⼼心API" },
  { stage: "React", title: "组件化1" },
  { stage: "React", title: "组件化2" }
];
// ⾼高阶组件withContent负责包装传⼊入组件Comp
// 包装后组件能够根据传⼊入索引获取课程数据，真实案例例中可以通过api查询 得到
const withContent = Comp => props => {
  const content = lessons[props.idx];
  // {...props}将属性展开传递下去
  return <Comp {...content} />;
};

// LessonWithContent是包装后的组件
const LessonWithContent = withContent(Lesson);
export default function HocTest() {
  // HocTest渲染三个LessonWithContent组件 
  return (
    <div>
      {[0, 0, 0].map((item, idx) => (<LessonWithContent idx={idx} key={idx} />
      ))}
    </div>
  );
}
```

范例:改造前面案例使上下文使⽤更优雅

```js

// withConsumer是⾼高阶组件⼯工⼚厂，它能根据配置返回⼀一个⾼高阶组件 
function withConsumer(Consumer) {
  return Comp => props => {
    return <Consumer>
      {value => <Comp {...value} {...props} />}
    </Consumer>;
  };
}
// Child显示计数器器，并能修改它，多个Child之间需要共享数据
// 新的Child是通过withConsumer(Consumer)返回的⾼高阶组件包装所得 
const Child = withConsumer(Consumer)(function (props) {
  return (
    <div 
      onClick={() => props.add()} 
      title={props.name}
    >
      {props.counter}
    </div>
  )
}

export default class ContextTest extends React.Component {
  render() {
    return (
      <Provider value={{ counter: this.state.counter, add: this.add }}>
        {/* 改造过的Child可以⾃自动从Consumer获取值，直接⽤用就好了了*/}
        <Child name="foo" />
        <Child name="bar" />
      </Provider>
    );
  }
}
```

#### 链式调⽤

⾼阶组件最巧妙的⼀点，是可以链式调⽤。

```js
// ⾼阶组件withLog负责包装传⼊入组件Comp 
// 包装后组件在挂载时可以输出⽇日志记录 
const withLog = Comp => {
// 返回组件需要⽣生命周期，因此声明为class组件 
  return class extendseact.Component {
		render() {
			return <Comp {...this.props} />;
    }
    componentDidMount() {
      console.log("didMount", this.props);
    }
   };
};
// LessonWithContent是包装后的组件
const LessonWithContent = withLog(withContent(Lesson));
```

#### 装饰器写法

高阶组件本身是对装饰器模式的应用，⾃然可以利用ES7中出现的装饰器语法来更更优雅的书写代码。

CRA项目中默认不支持js代码使⽤装饰器语法，可修改后缀名为tsx则可以直接⽀持

```js
// 装饰器器只能⽤用在class上 // 执⾏行行顺序从下往上 
@withLog
@withContent
class Lesson2 extends React.Component { 
  render() {
    return (
      <div>
				{this.props.stage} - {this.props.title} </div>
		);
	}
}
export default function HocTest() { 
  // 这⾥里里使⽤用Lesson2
	return (
    <div>
    {[0, 0, 0].map((item, idx) => ( <Lesson2 idx={idx} key={idx} />))}
    </div>
	);
}
```

### 2、组件复合 - Composition

复合组件给与你足够的敏捷去定义⾃定义组件的外观和行为，这种方式更明确和安全。如果组件间有公用的非UI逻辑，将它们抽取为JS模块导⼊使 ⽤而不是继承它。

#### 组件复合

范例:Dialog组件负责展示，内容从外部传入即可， components/Composition.js

```js
 
import React from "react";
// Dialog定义组件外观和⾏行行为 
function Dialog(props) {
	return <div style={{ border: "1px solid blue" }}> {props.children}</div>;
}
export default function Composition() {
  return (
    <div>
   	 	{/* 传⼊入显示内容 */} 
      <Dialog>
        <h1>组件复合</h1>
        <p>复合组件给与你⾜足够的敏敏捷去定义⾃自定义组件的外观和⾏行行为</p>
      </Dialog>
    </div>
	);
}
```

