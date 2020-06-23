### Hooks

Hook是React16.8一个新增项，它可以让你在不编写 class 的情况下使用 state以及其他的 React 特性。

Hook的特点:

- 使你在⽆需修改组件结构的情况下复用状态逻辑

- 可将组件中相互关联的部分拆分成更小的函数，复杂组件将变得更更容易理解

- 更简洁、更易理解的代码

```bash
 npm i react react-dom -S
```

#### 状态钩⼦子 State Hook

- 创建HooksTest.js

  ```js
  import React, { useState } from "react";
  export default function HooksTest() {
  // useState(initialState)，接收初始状态，返回⼀一个由状态和其更更新函数组成的数组
  const [fruit, setFruit] = useState(""); 
    return (
      <div>
        <p>{fruit === "" ? "请选择喜爱的⽔水果:" : `您的选择是: ${fruit}`}</p>
  		</div>
  	); 
  }
  ```

  

- 声明多个状态变量

  ```js
  // 声明列列表组件
  function FruitList({fruits, onSetFruit}) {
    return ( 
      <ul>
    	{fruits.map(f => (<li key={f} onClick={() => onSetFruit(f)}>{f} </li>)
      	)
  		}
    	</ul>
    ); 
  }
  export default function HooksTest() {
  // 声明数组状态
  const [fruits, setFruits] = useState(["⾹香蕉", "⻄西⽠瓜"]); 
    return (
  		<div>
        {/*添加列列表组件*/}
        <FruitList fruits={fruits} onSetFruit={setFruit}/>
      </div>
  	);
  }
  ```

- 用户输⼊处理

  ```js
  // 声明输⼊入组件
  function FruitAdd(props) {
    // 输⼊入内容状态及设置内容状态的⽅方法
    const [pname, setPname] = useState(""); // 键盘事件处理理
    const onAddFruit = e => {
      if (e.key === "Enter") { props.onAddFruit(pname); setPname("");} 
    };
    return (
        <div>
          <input
            type="t
            value={pname}
            onChange={e => setPname(e.target.value)} 
            onKeyDown={onAddFruit}
          /> 
        </div>
    );
  }
  export default function HooksTest() { // ...
    return (
      <div>
        {/*添加⽔水果组件*/}
        <FruitAdd onAddFruit={pname => setFruits([...fruits, pname])} />
   		</div>
  	);
  }
  ```

### 副作用钩子 Effect Hook

useEffect 给函数组件增加了执行副作用操作的能力。

副作用(Side Effect)是指⼀个 function 做了和本身运算返回值⽆关的 事，⽐如:修改了全局变量、修改了传⼊的参数、甚⾄是 console.log()， 所以 ajax 操作，修改 dom 都是算作副作用。

- 异步数据获取，更新HooksTest.js

  ```js
  import { useEffect } from "react";
  useEffect(()=>{ 
    setTimeout(() => {
  		setFruits(['⾹香蕉','⻄西⽠瓜']) 
    }, 1000);
  })
  ```

- 设置依赖

  ```js
  // 设置空数组意为没有依赖，则副作⽤操作仅执行一次 
  useEffect(()=>{...}, [])
  ```

  >
  >
  >如果副作⽤用操作对某状态有依赖，务必添加依赖选项
  >
  >```js
  > useEffect(() => { 
  >   document.title = fruit;
  >}, [fruit]);
  >```

- 清除工作:有⼀些副作用是需要清除的，清除⼯作非常重要的，可以防止引起内存泄露

  ```js
  useEffect(() => {
  	const timer = setInterval(() => {
  		console.log('msg'); 
    }, 1000);
    return function(){
        clearInterval(timer);
  	}
  }, []);
  ```

  

#### useReducer

useReducer是useState的可选项，常⽤于组件有复杂状态逻辑时，类似于 redux中reducer概念。

- 商品列表状态维护

  ```js
  import { useReducer } from "react";
  // 添加fruit状态维护fruitReducer 
  function fruitReducer(state, action) {
    switch (action.type) {
      case "init":
        return action.payload; case "add":
  
        return [...state, action.payload]; default:
        return state;
    }
  }
  export default function HooksTest() {
    // 组件内的状态不不需要了了
    // const [fruits, setFruits] = useState([]);
    // useReducer(reducer，initState)
    const [fruits, dispatch] = useReducer(fruitReducer, []);
    useEffect(() => {
      setTimeout(() => {
        // setFruits(["⾹香蕉", "⻄西⽠瓜"]);
        // 变更更状态，派发动作即可
        dispatch({
          type: "init", payload: ["⾹香蕉", "⻄西⽠瓜"]
        });
      }, 1000);
    }, []);
    return (
      <div>
        {/*此处修改为派发动作*/}
        <FruitAdd onAddFruit={pname => dispatch({ type: 'add', payload: pname })} />
      </div>
    );
  }
  ```

  

#### useContext

useContext⽤于在快速在函数组件中导入上下⽂。

```js
import React, { useContext } from "react"; // 创建上下⽂文
const Context = React.createContext();
export default function HooksTest() { 
  return (
    {/* 提供上下⽂文的值 */ }
    <Context.Provider value={{ fruits, dispatch }}>
      <div>
      {/* 这⾥里里不不再需要给FruitAdd传递状态mutation函数，实现了了解 耦 */}
        <FruitAdd />
      </div>
    </Context.Provider>
  ); 
}
function FruitAdd(props) {
  // 使⽤用useContext获取上下⽂文
  const { dispatch } = useContext(Context)
  const onAddFruit = e => {
    if (e.key === "Enter") {
      // 直接派发动作修改状态
      dispatch({ type: "add", payload: pname }) 
      setPname("");
    }
  };
  // ...
}
```

