## 什么是webpack

Webpack可以看做是模块打包机:它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的⼀些浏览器器不能直接运行的拓展语⾔(Scss，TypeScript等)，并将其打包为合适的格式以供浏览器使用。

### 1. 安装

最好使用局部安装，不推荐使用全局安装

```bash
npm install webpack webpack-cli --save-dev //-D
```

查看webpack的历史发布信息

```bash
 npm info webpack//查看webpack的历史发布信息
```

检查安装是否成功

```bash
webpack -v  //command not found 默认在全局环境中查找
npx webpack -v // npx帮助我们在项⽬目中的node_modules⾥查找
webpack ./node_modules/.bin/webpack -v //到当前的node_modules模块⾥里里指定 webpack
```

测试： 启动webpack打包

```js
// es moudule 模块引⼊入 
// commonJs 模块引⼊入
import add from "./a"; //需要使⽤用es moudule导出 
import minux from "./b";////需要使⽤用es moudule导出

npx webpack //打包命令 使⽤用webpack处理index.js这个⽂文件
```

总结:webpack 是一个模块打包工具，可以识别出引入模块的语法 ，早期的webpack只是个js模块的打包工具，现在可以是css，png，vue的模块打包⼯具

### 2. Webpack配置文件

零配置是很弱的，特定的需求，总是需要⾃己进行配置

```js
module.exports = {
  entry: "./src/index.js",//默认的⼊入⼝口⽂文件 
  output: "./dist/main.js"//默认的输出
};
```

当我们使用npx webpack,，表示的是使用webpack处理打包，./src/index.js的⼊口模块。默认放在当前目录下的dist目录，打包后的模块名称是main.js，当然我们也可以修改
webpack有默认的配置文件，叫webpack.config.js，我们可以对这个文件进行修改，进行个性化配置

- 默认的配置⽂件:webpack.config.js

  ```bash
  npx webpack //执⾏行行命令后，webpack会找到默认的配置文件，并使执行
  ```

- 不使⽤默认的配置文件: webpackconfig.js

  ```bash
  npx webpack --config webpackconfig.js //指定webpack使⽤webpackconfig.js⽂件来作为配置文件并执⾏
  ```

- 修改package.json scripts字段:有过vue react开发经验的同学 习惯使用npm run来启动，我们也可以修改下，原理就是模块局部安装会在 **node_modules/.bin**⽬录下创建⼀个软链接。

  ```bash
  "scripts":{ 
  	"bundle":"webpack"
  }
  npm run bundle
  ```

**Webpack.config.js配置结构**

```js
 
module.exports = {
  entry: "./src/index.js", //打包⼊入⼝口⽂文件 output: "./dist", //输出结构
  mode: "production", //打包环境
  module: {
  	rules: [ 
      //loader模块处理理 
      {
        test: /\.css$/,
        use: "style-loader"
      }
    ]
 	},
	plugins: [new HtmlWebpackPlugin()] //插件配置 
};
```

**项目结构优化**

```
dist 
	//打包后的资源⽬目录
node_modules 
	//第三⽅方模块
src 
	//源代码
	css
	images
	index.js
package.json
webpack.config.js
```

### 3. Webpack的核心概念

**entry**

指定webpack打包⼊口文件:Webpack 执行构建的第⼀步将从 Entry 开 始，可抽象成输⼊

```js
//单⼊入⼝口 SPA，本质是个字符串串 
entry:{
	main: './src/index.js' 
}
==相当于简写=== 
entry:"./src/index.js"
//多⼊入⼝口 entry是个对象 
entry:{
	index:"./src/index.js",
	login:"./src/login.js" 
}
```

**output**

打包转换后的⽂件输出到磁盘位置:输出结果，在 Webpack 经过⼀系列处理并得出最终想要的代码后输出结果。

```js
output: {
	filename: "bundle.js",//输出⽂文件的名称
	path: path.resolve(__dirname, "dist")//输出⽂文件到磁盘的⽬目录，必须是绝对路路径
},
//多⼊口的处理
output: {
	filename: "[name][chunkhash:8].js",//利用占位符，⽂件名称不要重复
	path: path.resolve(__dirname, "dist")//输出文件到磁盘的⽬录，必须是绝对路径
},
```

**mode**

Mode用来指定当前的构建环境

production | development | none

设置mode可以⾃动触发webpack内置的函数，达到优化的效果

<img src="/Users/andcool/Documents/workspace/Javascript-Note/JavaScript/React/images/image-20200708231538077.png" alt="image-20200708231538077" style="zoom:67%;" />

开发阶段的开启会有利于热更新的处理，识别哪个模块变化

⽣产阶段的开启会有帮助模块压缩，处理副作⽤等一些功能

**loader**

模块解析，模块转换器，⽤于把模块原内容按照需求转换成新内容。

webpack是模块打包工具，⽽模块不仅是js，还可以是css，图⽚或者其 他格式

但是webpack默认只知道如何处理理**js**和**JSON**模块，那么其他格式的模块处 理理，和处理方式就需要loader了

常见的loader

```js
style-loader
css-loader
less-loader
sass-loader
ts-loader //将Ts转换成js 
babel-loader//转换ES6、7等js新特性语法 
file-loader//处理理图⽚片⼦子图 
eslint-loader
...
```

**module**

模块，在 Webpack ⾥一切皆模块，⼀个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。

```js
module:{
  rules:[
    {
      test:/\.xxx$/,//指定匹配规则 
      use:{
      	loader: 'xxx-load'//指定使⽤用的loader 
      }
    }
  ] 
}
```

当webpack处理到不认识的模块时，需要在webpack中的module处进行配置，当检测到是什么格式的模块，使⽤什么loader来处理。

- loader: file-loader:处理理静态资源模块

  loader: file-loader

  原理是把打包⼊口中识别出的资源模块，移动到输出目录，并且返回⼀个地址名称

  

  所以我们什么时候⽤file-loader呢? 

  场景:就是当我们需要模块，仅仅是从源代码挪移到打包⽬录，就可以使⽤file-loader来处理，txt，svg，csv，excel，图片资源啦等

  ```bash
  npm install file-loader -D
  ```

案例

```js
 
module: {
  rules: [
    {
      test: /\.(png|jpe?g|gif)$/, //use使⽤⼀个loader可以用对象，字符串，两个loader需要用数组
      use: {
        loader: "file-loader",
        // options额外的配置，⽐比如资源名称 options: {
        // placeholder 占位符 [name]⽼老老资源模块的名称 // [ext]⽼老老资源模块的后缀
        // https://webpack.js.org/loaders/file-loader#placeholders
        name: "[name]_[hash].[ext]", 
        //打包后的存放位置
        outputPath: "images/",
        publicPath: "../images"
      }
    } 
    }
  ]
},
```

```js
import pic from "./logo.png";

var img = new Image(); 
img.src = pic; 
img.classList.add("logo");

var root = document.getElementById("root"); 
root.append(img);
 
```

- 处理理字体 https://www.iconfont.cn/?spm=a313x.7781069.1998910419.d4d0a486a

```css
//css
@font-face {
  font-family: "webfont";
  font-display: swap;
  src: url("webfont.woff2") format("woff2");
}
body {
  background: blue;
  font-family: "webfont" !important;
}
//webpack.config.js {
  test: /\.(eot|ttf|woff|woff2|svg)$/,
  use: "file-loader" 
}
```

- url-loader file-loader加强版本

  url-loader内部使用了file-loader,所以可以处理file-loader所有的事情，但是遇到jpg格式的模块，会把该图⽚片转换成base64格式字符串， 并打包到js里。对小体积的图⽚比较合适，⼤图⽚不合适。

  ```bash
  npm install url-loader -D
  ```

  案例

  ```js
   
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif)$/, 
        use: {
          loader: "url-loader", 
          options: {
            name: "[name]_[hash].[ext]", 
            outputPath: "images/", //⼩小于2048，才转换成base64 
            limit: 2048
          }
        }
      }
    ]
  },
  ```

- 样式处理

  Css-loader 分析css模块之间的关系，并合成一个css
  Style-loader 会把css-loader⽣成的内容，以style挂载到页面的heade部分

  ```bash
  npm install style-loader css-loader -D
  ```

  ```js
  {
    test: /\.css$/,
    use: ["style-loader", "css-loader"]
  }
  ```

  Less样式处理
  less-load 把less语法转换成css

  ```bash
  npm install less less-loader --save-dev
  ```

  案例:

  loader有顺序，从右到左，从下到上

  ```js
  {
    test: /\.scss$/,
    use: ["style-loader", "css-loader", "less-loader"]
  }
  ```

  样式自动添加前缀:

  https://caniuse.com/

  Postcss-loader

  ```bash
  npm i postcss-loader autoprefixer -D
  ```

  webpack.config.js

  ```js
  {
    test: /\.less$/,
  	use: [ "style-loader", "css-loader", "less-loader", {
  		loader: "postcss-loader", 
      options: {
  			plugins: [ 
          require("autoprefixer")({
  					overrideBrowserslist: ["last 2 versions",">1%"]
  				})
  			]
      }
  	}]
  },
  ```

  或者: 新建postcss.config.js

  ```js
  //webpack.config.js
  {
  	test: /\.css$/,
  	use: ["style-loader", "css-loader", "postcss-loader"]
  },
  //postcss.config.js
  module.exports = { 
    plugins: [
  		require("autoprefixer")({
  			overrideBrowserslist: ["last 2 versions", ">1%"]
  		})
    ]
  };
  ```

**Plugins**

plugin 可以在webpack运行到某个阶段的时候，帮你做⼀些事情，类似于⽣命周期的概念

扩展插件，在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情。

作⽤于整个构建过程

- HtmlWebpackPlugin

  htmlwebpackplugin会在打包结束后，⾃动⽣成一个html⽂文件，并把打包 ⽣成的js模块引⼊入到该html中。

  ```bash
   npm install --save-dev html-webpack-plugin
  ```

  配置

  ```
   
  title: ⽤来⽣成⻚面的 title 元素
  
  filename: 输出的 HTML ⽂件名，默认是 index.html, 也可以直接配置带有⼦目录。
  
  template: 模板⽂件路径，⽀持加载器，⽐比如 html!./index.html inject: true | 'head' | 'body' | false ,注⼊所有的资源到特定 的 template 或者 templateContent 中，如果设置为 true 或者 body，所有的 javascript 资源将被放置到 body 元素的底部，'head' 将 放置到 head 元素中。
  
  favicon: 添加特定的 favicon 路径到输出的 HTML ⽂件中。
  
  minify: {} | false , 传递 html-minifier 选项给 minify 输出 hash: true | false, 如果为 true, 将添加⼀个唯一的 webpack 编译 hash 到所有包含的脚本和 CSS 文件，对于解除 cache 很有用。
  
  cache: true | false，如果为 true, 这是默认值，仅在文件修改之后 才会发布文件。
  
  showErrors: true | false, 如果为 true, 这是默认值，错误信息会写入到 HTML ⻚面中
  
  chunks: 允许只添加某些块 (⽐比如，仅仅 unit test 块) 
  
  chunksSortMode: 允许控制块在添加到⻚面之前的排序⽅式，⽀持的值:'none' | 'default' | {function}-default:'auto' 
  
  excludeChunks: 允许跳过某些块，(⽐如，跳过单元测试的块)
   
  ```

  案例

  ```js
   
  const path = require("path");
  const htmlWebpackPlugin = require("html-webpack-plugin");
  module.exports = {
  	plugins: [
      new htmlWebpackPlugin({
        title: "My App",
        filename: "app.html", template: "./src/index.html"
        })
    ]
  };
  ```

- clean-webpack-plugin

  ```bash
  npm install --save-dev clean-webpack-plugin
  ```

  ```js
  const { CleanWebpackPlugin } = require("clean-webpack- plugin");
  plugins: [
      new CleanWebpackPlugin()
  ]
  ```

- mini-css-extract-plugin

  ```js
  const MiniCssExtractPlugin = require("mini-css-extract- plugin");
  {
    test: /\.css$/,
    use: [MiniCssExtractPlugin.loader, "css-loader"]
  }
  new MiniCssExtractPlugin({
    filename: "[name][chunkhash:8].css"
  })
  ```

**sourceMap**

源代码与打包后的代码的映射关系，通过sourceMap定位到源代码。

在dev模式中，默认开启，关闭的话可以在配置⽂件⾥

```js
devtool:"none"
```

devtool的介绍:https://webpack.js.org/configuration/devtool#devtool

eval:速度最快,使⽤用eval包裹模块代码,

source-map: 产⽣生.map文件

cheap:较快，不包含列信息

Module:第三⽅模块，包含loader的sourcemap(⽐比如jsx to js ，babel 的sourcemap)

inline: 将.map作为DataURI嵌⼊，不单独生成.map⽂件

配置推荐:

> devtool:"cheap-module-eval-source-map",// 开发环境配置
>
> //线上不推荐开启
>  devtool:"cheap-module-source-map", // 线上⽣成配置

**WebpackDevServer**

提升开发效率的利器 每次改完代码都需要重新打包⼀次，打开浏览器，刷新⼀次，很麻烦 我们可以安装使用webpackdevserver来改善这块的体验

启动服务后，会发现dist⽬录没有了，这是因为devServer把打包后的模块不会放在dist目录下，而是放到内存中，从⽽提升速度

```bash
 npm install webpack-dev-server -D
```

修改下package.json

```js
"scripts": {
	"server": "webpack-dev-server"
},
```

在webpack.config.js配置:

```js
devServer: {
	contentBase: "./dist",
  open: true,
	port: 8081
},
```

跨域: 

联调期间，前后端分离，直接获取数据会跨域，上线后我们使用nginx转发，开发期间，webpack就可以搞定这件事

启动一个服务器器，mock⼀个接口

```js
// npm i express -D
// 创建一个server.js 修改scripts "server":"node server.js"
//server.js
const express = require('express')
const app = express() app.get('/api/info', (req,res)=>{
  res.json({ name:'开课吧', age:5, msg:'欢迎来到开课吧学习前端⾼高级课程'})
}) app.listen('9092')
//node server.js 
http://localhost:9092/api/info
```

项⽬中安装axios工具

```js
//npm i axios -D
//index.js
import axios from 'axios' 
axios.get('http://localhost:9092/api/info').then(res=>{
	console.log(res) 
})
会有跨域问题
```

修改webpack.config.js 设置服务器代理

```js
proxy: { 
  "/api": {
		target: "http://localhost:9092"
  }
}
```

修改index.js

```js
axios.get("/api/info").then(res => { 
  console.log(res);
});
```

搞定! 

> 1. 和服务端约定好接口!!!!!，定义好字段!!!!!
>
> 2. 接⼝文档啥时候给到。 
>
> 3. 根据接⼝文档mock数据，mock接⼝

**⽂件监听**

轮询判断⽂件的最后编辑时间是否变化，某个⽂件发生了变化，并不会⽴刻告诉监听者，先缓存起来

webpack开启监听模式，有两种

```js
1.启动webpack命令式 带上--watch 参数，启动监听后，需要⼿手动刷新浏览 器器

scripts:{
	"watch":"webpack --watch"
}
2.在配置⽂文件⾥里里设置 watch:true

watch: true, //默认false,不不开启 
//配合watch,只有开启才有作⽤用 
watchOptions: {
  //默认为空，不不监听的⽂文件或者⽬目录，⽀支持正则
  ignored: /node_modules/, 
  //监听到⽂文件变化后，等300ms再去执⾏行行，默认300ms, 
  aggregateTimeout: 300, 
  //判断⽂文件是否发⽣生变化是通过不不停的询问系统指定⽂文件有没有变化，默认每秒问1次
	poll: 1000 //ms
}
 
```

