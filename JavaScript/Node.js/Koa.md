# Koa

### 概念

koa 是⼀个新的 **web** 框架， 致力于成为 **web** 应⽤和 **API** 开发领域中的一个更小、更富有表现力、更健壮的基⽯。

> Express是一个很轻便的框架，集成了http，static这些服务；
>
> koa比Express更精简，它把这些服务都拆分成了中间件，一切是以中间件为基础。因为js的异步操作，使得回调陷阱的写法一直没有得到改善，直到es6出来以后，有了generator，yield语法之后这个使用才顺起来，为了提供支持相应语法，就开发了koa；
>
> koa2是es7出来以后，完全使用Promise并配合async/await来实现异步；

**异步操作最难的地方是异步操作的串行处理**，如果没有Promise，async/await这样的支持，就会很麻烦

### 特点

- 轻量、无捆绑
- 中间件架构，无捆绑
- 优雅的API设计
- 增强的错误处理

### 中间件

Koa中间件机制：Koa中间件机制就是函数组合的概念，将一组需要顺序执行的函数复合为一个函数，外层函数的参数实际是内层函数的返回值。洋葱圈模型可以形象表示这种机制，是源码中的精髓和难点。

Koa中间件的使用**即能够满足业务逻辑的顺序调用的需要，又能够满足对切面编程的需要**。原因是想在哪里做切面编程，既可以在某一个中间件中完成，也可以编写一个中间件完成。

<img src="/Users/andcool/Documents/workspace/Javascript-Note/JavaScript/Node.js/images/view.png" alt="preview" style="zoom:67%;" />

<img src="/Users/andcool/Documents/workspace/Javascript-Note/JavaScript/Node.js/images/view2.png" alt="preview" style="zoom:67%;" />

### Context

Koa为了简化API，引入上下文context概念，将原始请求对象req和响应对象res封装并挂载到context上，并且在context上设置getter和setter，从而简化操作。









### 1、简单的http服务是可以完成请求的，为什么还要使用web服务框架？web服务框架都解决了什么问题？

vue解决的了MVVM的问题，如果不使用这个框架，那开发一个页面就会非常麻烦

后段欠缺什么呢？原生的http服务欠缺什么呢？

一是令人困惑的request和response，res.end 是一个流（流是什么东西），koa中使用ctx.body就可以完成请求，使用res.end就让人很困惑返回的是一个什么东西，还有res.writeHeader也会让人很困惑是做什么。当然这其中的原因不得而知，作为一个http的请求服务，尽可能覆盖到大而全的语言调用，在设计的时候会尽可能的考虑到最底层的实现方式，他不会对应用层考虑太多。所以需要做一些优雅的API来封装http去使用。

二是作为一个web服务框架，如何描述复杂业务逻辑，其中包括两点，一个是流程顺序的描述，第二是切面描述AOP（语言级，框架级），比如登陆鉴权，全局日志记录...这些该如何去优雅操作

**问题：vue，react，axios，redux中哪些部分使用了切面编程？**
vue和react的生命周期就是一种切面，axios的intercepter，redux的一些插件，vue和react中router提供的守卫，哨兵...都是使用切面编程，每个框架都会有这方面的考量。当然对于登陆鉴权，日志记录是可以在代码多处去写代码记录的，但是最好的方式就是一处编写，处处执行，这就是切面编程的优点。



### 2、Koa中next()使用了什么设计模式





---

### 1、 自己封装一个Koa框架

```js
const Koa = require('./MyKoa')
const app = new Koa()

app.use((req, res) => {
  res.writeHead(200)
  res.end('hi koa')
})

app.listen(3000, () => {
  console.log(`监听端口3000`)
})
```

```js
const http = require('http')
class MyKoa {
  listen(...args) {
    const server = http.createServer((req, res) => {
      this.callback(req, res)
    })
    server.listen(...args)
  }

  use(callback) {
    this.callback = callback
  }
}
module.exports = MyKoa
```

### 2、自己实现context

```js
index.js

const Koa = require('./MyKoa')
const app = new Koa()

app.use((ctx, next) => {
  ctx.body = 'hehe'
})

app.listen(3000, () => {
  console.log(`监听端口3000`)
})
--------------------------------------
MyKoa.js

const http = require('http')
const context = require('./context')
const request = require('./request')
const response = require('./response')

class MyKoa {
  listen(...args) {
    const server = http.createServer((req, res) => {
      // 创建上下文环境
      const ctx = this.createContext(req, res)
      this.callback(ctx)
      res.end(ctx.body)
    })
    server.listen(...args)
  }

  use(callback) {
    this.callback = callback
  }
  // 构建上下文
  createContext(req, res) {
    const ctx = Object.create(context)
    ctx.request = Object.create(request)
    ctx.response = Object.create(response)

    ctx.req = ctx.request = req
    ctx.res = ctx.response = res
    return ctx
  }
}
module.exports = MyKoa
--------------------------------------
request.js

module.exports = {
  get url() {
    return this.req.url;
  },
  get method() {
    return this.req.method.toLowerCase()
  }
};
--------------------------------------
response.js

module.exports = {
  get body() {
    return this._body;
  },
  set body(val) {
    this._body = val;
  }
};
--------------------------------------
context.js

module.exports = {
  get url () {
    return this.request.url
  },
  get body() {
    return this.response.body
  },
  set body(value) {
    this.response.body = value
  },
  get method() {
    return this.request.method
  }
}
```

