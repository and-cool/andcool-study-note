# Jest 使用axios发送请求测试数据

## Error Info

``` 
Error: Cross origin http://localhost forbidden
          at dispatchError (/Users/andcool/Documents/xukeTech-beijing/git-project/thor/node_modules/jsdom/lib/jsdom/living/xhr-utils.js:65:19)
          at Object.validCORSHeaders (/Users/andcool/Documents/xukeTech-beijing/git-project/thor/node_modules/jsdom/lib/jsdom/living/xhr-utils.js:77:5)
          at receiveResponse (/Users/andcool/Documents/xukeTech-beijing/git-project/thor/node_modules/jsdom/lib/jsdom/living/xmlhttprequest.js:847:21)
          at Request.client.on.res (/Users/andcool/Documents/xukeTech-beijing/git-project/thor/node_modules/jsdom/lib/jsdom/living/xmlhttprequest.js:679:38)
          at Request.emit (events.js:198:13)
          at Request.onRequestResponse (/Users/andcool/Documents/xukeTech-beijing/git-project/thor/node_modules/request/request.js:1059:10)
          at ClientRequest.emit (events.js:203:15)
          at HTTPParser.parserOnIncomingClient [as onIncoming] (_http_client.js:556:21)
          at HTTPParser.parserOnHeadersComplete (_http_common.js:109:17)
          at Socket.socketOnData (_http_client.js:442:20) undefined
```

出现这个错误也很正常。

前后端分离，域名和端口不同，故存在跨域

但是前端项目和后端项目都是在本地运行的，理论上不存在跨域等问题，那是什么原因导致的呢？

研究了一下请求发送的方式，通过axios提交发送请求，请求的最终发送者是Jest，Jest发送请求默认的是在浏览器环境下发送的请求，官方写的是jsdom，同样我们可以选择在node环境下运行测试

```
testEnvironment [string]
Default: "jsdom"

The test environment that will be used for testing. The default environment in Jest is a browser-like environment through jsdom. If you are building a node service, you can use the node option to use a node-like environment instead.

By adding a @jest-environment docblock at the top of the file, you can specify another environment to be used for all tests in that file:

```

```
/**
 * @jest-environment jsdom
 */

test('use jsdom in this test file', () => {
  const element = document.createElement('div');
  expect(element).not.toBeNull();
});
```

So, 现在问题就好解决了，我们可以设置 ```jest-environment``` 为node就可以正常发送数据了

后端会设置```Access-Control-Allow-Origin```所以会限制请求的Origin，所以如果在浏览器中如果没有以正确的域名和端口号请求后端就会报错，而jsdom环境下，Jest会以http://localhost 为Origin去请求后端，所以如果后端的```Access-Control-Allow-Origin```不等于http://localhost的话就会出现CROS问题。

为什么改为node环境就正常了呢？

因为在node环境下是以server的方式请求后端，在同一台机器上运行是不存在跨域的问题。