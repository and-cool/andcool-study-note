# React全家桶及技术原理总结

## react-router

React Router 包含三个库

- react-router
- react-router-dom
- react-router-native

react-router 提供最基本的路路由功能，实际使用，我们不会直接安装 react-router,⽽是根据应⽤运行的环境选择安装 react-router- dom(在浏览器器中使⽤用)或 react-router-native(在 react-native中使⽤用)。react-router-dom 和 react-router-native 都依赖 react-router,所以在安装 时, react-router 也会⾃自动安装。 创建 Web应⽤使用。

react-router中奉⾏一切皆组件的思想，路由器-**Router**、链接-**Link**、路由-**Route**、独占-**Switch**、重定向-**Redirect**都以组件形式存在

React Router 通过 Router 和 Route 两个组件完成路路由功能，在 Web应⽤用 中，我们⼀一般会使⽤对 Router 进⾏包装的 BrowserRouter 或 HashRouter 两个组件， BrowserRouter使⽤HTML5 的 history API(pushState、replaceState等)实现应用的 UI 和 URL 的同步。

**创建RouterTest.js**

```javascript
 import React from "react";
import { BrowserRouter, Link, Route } from "react-router- dom";
function ProductList(props) {
  return <div>ProductList</div>;
}
function ProductMgt(props) {
  return <div>ProductMgt</div>;
}

export default function RouterTest() {
  return (
    <BrowserRouter>
      <nav>
        {/* 导航 */}
        <Link to="/">商品列列表</Link>
        <Link to="/management">商品管理理</Link>
      </nav> <div>
        {/* 直接在组件中定义路路由 */}
        {/* 根路路由要添加exact，render可以实现条件渲染 */}
        <Route exact path="/" component={ProductList} />
        <Route path="/management" component={ProductMgt} />
      </div>
    </BrowserRouter>
  );
}
```

### 动态路由

使⽤:id的形式定义动态路由 

定义路由，RouterTest

```javascript
<Route path="/detail/:name" component={Detail} />
```
添加导航链接，ProductList
```javascript
<Link to="/detail/web">web全栈</Link>
```
创建Detail组件并获取参数
```javascript

function Detail({ match, history, location }) {
  console.log(match, history, location);
  return (
    <div>
      ProductMgt
      <p>{match.params.name}</p>
    </div>
  );
}
```

### 路由嵌套

Route组件嵌套在其他⻚面组件中就产⽣了嵌套关系

修改ProductMgt，添加新增和搜索商品

```javascript
function ProductMgt(props) {
  return <div>
    <h3>ProductMgt</h3>
    <Link to="/management/add">新增商品</Link>
    <Link to="/management/search">搜索商品</Link>
    <Route 
      path="/management/add" 
      component={() =>
        <div>add</div>}
    />
    <Route 
      path="/management/search" 
      component={() =>
        <div>search</div>}
    />
  </div>;
}
```

### 动态引入 **&&** 基于路由的代码分割

**React.lazy **接受一个函数，这个函数需要动态调用 **import()**。它必须 返回⼀一个 **Promise** ，该 Promise 需要 resolve ⼀一个 **defalut export** 的 React 组件。

然后应在** Suspense** 组件中渲染 lazy 组件，如此使得我们可以使⽤在等待 加载 lazy 组件时做优雅降级(如 loading 指示器等)。

**React.lazy** ⽬前只支持默认导出(default exports)

```javascript
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';
const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));
const App = () => (<Router>
  //fallback 属性接受任何在组件加载过程中你想展示的 React 元素。 
  <Suspense fallback={<div>Loading...</div>}>
    <Switch>
      <Route exact path="/" component={Home} /> <Route path="/about" component={About} />
    </Switch>
  </Suspense>
</Router>);
```

### 404页面

设定一个没有path的路由在路由列表最后面，表示⼀定匹配

```javascript
{/* 添加Switch表示仅匹配⼀一个 */ }
<Switch>
  {/* ⾸首⻚页重定向换成Route⽅方式处理理避免影响404 */}
  <Route exact path="/" render={props => <Redirect to="/list" />} />
  {/* <Redirect to="/list"></Redirect> */}
  <Route component={() => <h3>⻚页⾯面不不存在</h3>}></Route>
</Switch>
```

### 路由守卫

思路:创建高阶组件包装Route使其具有权限判断功能

创建PrivateRoute

```javascript
function PrivateRoute({ component: Component, isLogin, ...rest }) {
  // 单独解构出component和isLogin
  // component为渲染⽬目标组件，isLogin通常来⾃自Redux // rest为传递给Route的属性
  return (
    <Route
      {...rest} render={
        props => // props包含match等信息直接传给⽬目标组件 
          isLogin ? ( // 若登陆渲染⽬目标组件
            <Component {...props} />) : ( // 未登录重定向到Login
              <Redirect
                to={{
                  pathname: "/login",
                  state: {
                    redirect: props.location.pathname
                  } // 重定向地址
                }}
              />
            )
      }
    />
  );
}
```

创建Login
```javascript
 function Login({ location, isLogin, login }) {
  const redirect = location.state.redirect || "/"; // 重定向地址
  if (isLogin) return <Redirect to={redirect} />;
  return (
    <div>
      <p>⽤用户登录</p>
      <hr />
      <button onClick={login}>登录</button>
    </div>
  );
}
```

配置路由，ReduxTest

```javascript
 <PrivateRoute path="/management" component={ProductMgt} />
 <Route path="/login" component={Login} />
```

>给PrivateRoute传递isLogin={true}试试
>
>整合redux，获取和设置登录态，创建./store/user.js
>
>```javascript
>
>const initialState = { isLogin: false, loading: false };
>export default (state = initialState, action) => {
>  switch (action.type) {
>    case "requestLogin":
>      return { isLogin: false, loading: true };
>    case "loginSuccess":
>      return { isLogin: true, loading: false };
>    case "loginFailure":
>      return { isLogin: false, loading: false };
>    default:
>      return state;
>  }
>};
>export function login(user) {
>  return dispatch => {
>    dispatch({ type: "requestLogin" }); setTimeout(() => {
>      dispatch({ type: "loginSuccess" });
>    }, 1000);
>  };
>}
>```
>
>引⼊入，store/index.js
>
>```javascript
> 
>import user from "./user";
>const store = createStore( combineReducers({ user }), applyMiddleware(logger, thunk) );
>```
>
>连接状态，ReduxTest.js
>
>```javascript
>import { login } from "./store/user.redux";
>const PrivateRoute = connect(state => ({ isLogin: state.user.isLogin }))(function ({ isLogin, ...rest }) { })
>const Login = connect(state => ({
>  isLogin: state.user.isLogin
>}),
>  { login }
>)(function ({ isLogin, login }) { }) 
>```

### 自定义Router、Route、Link

AppRouter.js

```javascript
import React from 'react';
import './App.css';
import BrowserRouter from './MyRouter';
import MyRoute from './MyRoute';
import MyLink from './MyLink';

function Test(props) {
  console.log(props)
  return <h2>Test</h2>
}

function List(props) {
  console.log(props)
  return <h2>list</h2>
}

function Index(props) {
  console.log(props)
  return <h2>index</h2>
}

function AppRouter() {
  return (
   <BrowserRouter>
      <MyLink to="/index">首页</MyLink>
      <MyLink to="/list">列表</MyLink>
      {/* <MyRoute path="/home/:id" exact={false} component={Test}></MyRoute> */}
      <MyRoute path="/index" exact={false} component={Index}></MyRoute>
      <MyRoute path="/list" exact={false} component={List}></MyRoute>
   </BrowserRouter>
  );
}

export default AppRouter;

```



MyRouter.js

```javascript
import React from 'react';
import { createBrowserHistory } from 'history';
import { Provider } from './context';

class BrowserRouter extends React.Component {
  //创建原生的history对象
  constructor(props) {
    super(props)
    this.history = createBrowserHistory();
    console.log(this.history)
    this.state = {
      location: this.history.location
    };

    // 监听location，更新state.location
    this.unListen = this.history.listen((location) => {
      this.setState({
        location
      })
    })
  }

  componentWillUnmount() {
    if(this.unListen) {
      this.unListen()
    }
  }

  render() {
    let value = {
      history: this.history,
      location: this.state.location
    }
    return <Provider value={value}>
      {this.props.children}
    </Provider>
  }
}

export default BrowserRouter;
```


MyRoute.js

```javascript
import React, { Component } from 'react';
import pathToReg from 'path-to-regexp';
import { Consumer } from './context';
class MyRoute extends Component {
  constructor(props) {
    super(props);
    this.state = {  }
  }
  render() { 
    // <Route path="/" component={Test} exact={false} ></Route>
    return ( 
      <Consumer>
        {
          (value) => {
            let { path, component : Component, exact = false } = this.props;
            // 拿到url路径
            // 拿到path 然后比对（可以通过正则）
            let pathname = value.location.pathname;

            // 把path转换成正则表达式
            let keys = [];
             //生成正则表达式
            let reg = pathToReg(path, keys, {end: exact});

            keys = keys.map(item => {
              return item.name
            })
           
            // 如果匹配成功返回的是数组[]，如果匹配失败，返回为null, 有参数返回参数
            let result = pathname.match(reg);
            
            let [url, ...values] = result || [];

            console.log(keys, values)

            let props = {
              location: value.location,
              history: value.history,
              match: {
                params: keys.reduce((obj, current, index) => {
                  obj[current] = values[index];
                  return obj;
                }, {})
              }
            }

            if(result) {
              return <Component {...props} />
            }
            return null;
          }
        }
      </Consumer>
     );
  }
}
 
export default MyRoute;
```


MyLink.js

```javascript
import React, { Component } from 'react';
import { Consumer } from './context';
// Link就是a标签

// <Link to="nav">menu</Link>
class MyLink extends Component {
  constructor(props) {
    super(props);
    this.state = {  }
  }
  handleClick(event, history) {
    event.preventDefault();
    history.push(this.props.to);
  }
  render() {
    const { to, ...rest } = this.props;
    return (
      <Consumer>
        {
          (value) => {
            return (
              <a 
                {...rest}
                href={to} 
                onClick={event => { this.handleClick(event, value.history) }}>
                  {this.props.children}
              </a>
            )
          }
        }
      </Consumer>
     );
  }
}
 
export default MyLink;
```








