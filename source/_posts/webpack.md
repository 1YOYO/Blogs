---
title: webpack的搭建
---

  大半夜洗了个头，估计一时半会儿还睡不了觉。突然想到以后可能要用到webpack，今天就回顾一下webpack的搭建，方便自己以后用起来顺手些。
  主要步骤为：

> * 1.引入webpack依赖，全局安装webpack
> * 2.把运行命令配置到npm的script中(方便运行)
> * 3.常用加载器介绍(类似gulp插件)

------

>## 一、引入webpack依赖

### **1.1安装全局webpack**
在一个盘符中创建一个工程，（注意不要放在中文目录，以免不必要的麻烦）然后打开CMD，输入以下命令。

`npm install webpack -g`

### ***1.2创建配置文件***
在项目工程的根目录创建三个或多个webpack配置文件
（1）web.base.config.js  //公用的配置放在这里面
（2）web.develop.config.js  //开发环境中用到的配置文件
（3）web.publish.config.js   //生产环境中用到的配置文件


#### ***1.2.1在develop中配置基本内容***
```
var path = require('path');

module.exports = {
    entry:path.resolve(__dirname,'src/js/app.js'),//文件入口
    output: {                                     //输出文件配置信息
        path: path.resolve(__dirname, 'deploy/js'),
        filename: 'bundle.js',
    }
}
```
备注：这里说明一下，这里用到的时一种模块化的规范conmmonjs,想一行代码是导入，第三行是导出。
#### ***1.2.2运行检错***
 法一：在根目录运行webpack的命令，默认会去查找名称为webpack.config.js的文件执行，如果没有就会报配置信息没有配置的错误。。
 法二：还可以通过运行下面的命令进行要执行的配置文件的选择
webpack –-config  web.develop.config.js

。。。未完待续。。。（好吧，我承认我困得睁不开眼了，，睡觉去~）

>## 二、把运行命令配置到npm的script中

这里要说明一下，因为webpack的命令会很长，所以需要配置一下，方便运行。
#### ***2.1简单配置***
具体操作是：
        1. npm init --------------->会导出pakage.json
        2.这里需要安装webpack的局部变量：npm install webpack --save
        3.在pakeg.json中找到scripts，然后写:
`'develop' : 'webpack –-config web.develop.config.js'`
        4.测试运行：npm run develop
#### ***2.2为发布目录启动服务***
在这里我摘抄一段，便于理解：
> 如果需要一直输入 npm run develop 确实是一件非常无聊的事情，幸运的是，我们可以把让他安静的运行，让我们设置 webpack-dev-server
除了提供模块打包功能，Webpack还提供了一个基于Node.js Express框架的开发服务器，它是一个静态资源Web服务器，对于简单静态页面或者仅依赖于独立服务的前端页面，都可以直接使用这个开发服务器进行开 发。在开发过程中，开发服务器会监听每一个文件的变化，进行实时打包，并且可以推送通知前端页面代码发生了变化，从而可以实现页面的自动刷新。

1.安装webpack-dev-server   `npm install webpack-dev-server --save`
2.调整npm的package.json scripts 部分中开发命令的配置
```
 "scripts": {
 "develop": "webpack-dev-server  --config web.develop.config.js --devtool eval --progress --colors --hot --content-base src"
 }

```
备注：webpack-dev-server - 在 localhost:8080 建立一个 Web 服务器。
当你运行 npm run develop的时候，会启动一个Web服务器，然后监听文件修改，然后自动重新合并你的代码。真的非常简洁！
#### ***2.3 浏览器自动刷新***
在develop中的的入口文件前添加这两句：
`'webpack/hot/dev-server'` `'webpack-dev-server/client?http://localhost:8080',`
如：
```
module.exports = {
    entry:[
        'webpack/hot/dev-server',
        'webpack-dev-server/client?http://localhost:8080',
        path.resolve(__dirname,'src/js/app.js')
    ]
```
>##三、常用加载器介绍(类似gulp插件)--->摘抄

Loader：这是webpack准备的一些预处理工具
**3.1 编译jsx和ES6到原生js（将es6转化成浏览器能识别的解析的es5）**
3.1.1首先安装下面的所有依赖
npm install babel-loader --save-dev
npm install babel-core babel-preset-es2015 babel-preset-react --save-dev
3.1.2修改开发配置文件
```
module: {
    loaders: [
        {
            test: /\.jsx?$/, // 用正则来匹配文件路径，这段意思是匹配 js 或者 jsx
            loader: 'babel',// 加载模块 "babel" 是 "babel-loader" 的缩写
            query: {
                presets: ['es2015', 'react']
            }
        }
    ]
}

```
3.1.2使用
直接从新运行npm run develop即可
**3.2加载css**
Webpack允许像加载任何代码一样加载 CSS。你可以选择你所需要的方式，但是你可以为每个组件把所有你的 CSS 加载到入口主文件中来做任何事情。
加载 CSS 需要 css-loader 和 style-loader，他们做两件不同的事情，css-loader会遍历 CSS 文件，然后找到 url() 表达式然后处理他们，style-loader 会把原来的 CSS 代码插入页面中的一个 style 标签中。
3.2.1首先下载依赖
npm install css-loader style-loader --save-dev
3.2.2修改配置文件
{
 	test: /\.css$/, // Only .css files
	loader: 'style!css' // Run both loaders
}
！用来定义loader的串联关系，"-loader"是可以省略不写的，多个loader之间用“!”连接起来
3.2.3 Css加载策略
在你的主入口文件中，比如 src/app.js 你可以为整个项目加载所有的 CSS
	Import  './project-styles.css';
CSS 就完全包含在合并的应用中，再也不需要重新下载。
懒加载（推荐）
如果你想发挥应用中多重入口文件的优势，你可以在每个入口点包含各自的 CSS。
你把你的模块用文件夹分离，每个文件夹有各自的 CSS 和 JavaScript 文件。再次，当应用发布的时候，导入的 CSS 已经加载到每个入口文件中。
 
定制组件css
你可以根据这个策略为每个组件创建 CSS 文件，可以让组件名和 CSS 中的 class 使用一个命名空间，来避免一个组件中的一些 class 干扰到另外一些组件的 class。
 
	使用内联样式取代 CSS 文件
在 “React Native” 中你不再需要使用任何 CSS 文件，你只需要使用 style 属性，可以把你的 CSS 定义成一个对象，那样就可以根据你的项目重新来考略你的 CSS 策略。
注意：这点的样式都没有-
 
**3.3加载sass**
3.3.1下载依赖
npm install sass-loader  --save-dev
3.3.2修改配置文件


    {
     test: /\.scss$/,
     loader: 'style!css!sass'
    }

 
**3.4图片处理**
直到 HTTP/2 你才能在应用加载的时候避免设置太多 HTTP 请求。根据浏览器不同你必须设置你的并行请求数，如果你在你的 CSS 中加载了太多图片的话，可以自动把这些图片转成 BASE64 字符串然后内联到 CSS 里来降低必要的请求数，这个方法取决与你的图片大小。你需要为你的应用平衡下载的大小和下载的数量，不过 Webpack 可以让这个平衡十分轻松适应。
3.4.1下载依赖
npm install url-loader --save-dev
3.4.2修改配置文件
```
{
      test: /\.(png|jpg)$/,
      loader: 'url?limit=25000&name=images/[name].[ext]'
 }
```
加载器，它会把需要转换的路径变成 BASE64 字符串，在其他的 Webpack 书中提到的这方面会把你 CSS 中的 “url()” 像其他 require 或者 import 来处理。意味着如果我们可以通过它来处理我们的图片文件。
url-loader 传入的 limit 参数是告诉它图片如果不大于 25KB 的话要自动在它从属的 css 文件中转成 BASE64 字符串。
3.4.3大图片处理
在代码中是一下情况：
```
div.img{
    background: url(../image/xxx.jpg)
}

//或者
var img = document.createElement("img");
img.src = require("../image/xxx.jpg");
document.body.appendChild(img);
```
你可以这样配置
```
module: {
    {
      test: /\.(png|jpg)$/,
      loader: 'url-loader?limit=10000&name=build/[name].[ext]'
    }]
}
```
针对上面的两种使用方式，loader可以自动识别并处理。根据loader中的设置，webpack会将小于指点大小的文件转化成 base64 格式的 dataUrl，其他图片会做适当的压缩并存放在指定目录中。


**3.5文件处理**
3.5.1说明
现在webpack处理css中的图片还是可以的，但是处理html中的图片能力有限，所以有图片的地方最好放在css中，或者使用require的方式
 
3.5.2下载依赖
npm install file-loader –save-dev
3.5.2修改配置文件
```
{
    test: /\.(png|jpeg|gif|jpg)$/,
    loader: 'file-loader?name=images/[name].[ext]'
}
```
注意：这点由于一直加载不上图片，所以我把输出的配置改为了
```
output: {
    path: path.resolve(__dirname, 'deploy'),
    filename: 'bundle.js',
},
```
我把下面的loader注释了，影响图片的加载
```
    {
        test: /\.(png|jpg)$/,
        loader: 'url?limit=25000'
    }
    
```
**3.6内联fonts**
字体实在是非常难引入正确，首先，通常我们有 4 种不一样的格式，但是只有其中一种会被对应的浏览器使用到。你肯定不会想引入全部四种格式，这样只会让 CSS 文件更加膨胀，然后又没办法优化。
取决与你的项目，你可能可以选择出一种字体格式，如果你不考略 Opera Mini，所有的浏览器都支持 .woff 和 .svg 格式。问题是不同格式下在各种浏览器下字体看起来会有一点点不同。所以测试 .woff 和 .svg，然后找出能够在所有浏览器中看起来最好的那个。
{
      test: /\.woff$/,
      loader: 'url?limit=100000'
}


>## 四、常用插件介绍

webpack提供了[丰富的组件]用来满足不同的需求，当然我们也可以自行实现一个组件来满足自己的需求：
plugins: [
     //your plugins list
 ]
详细的请看这里：
http://webpack.github.io/docs/list-of-plugins.html#uglifyjsplugin
注释：Word中出现的所有的包，都可以通过npm进行包查找，然后查看具体的使用方法。
**4.1压缩插件**
这个插件是自带的
 
**4.2提取css插件**
在webpack中编写js文件时，可以通过require的方式引入其他的静态资源，可通过loader对文件自动解析并打包文件。通常会将js 文件打包合并，css文件会在页面的header中嵌入style的方式载入页面。但开发过程中我们并不想将样式打在脚本中，最好可以独立生成css文件，以外链的形式加载。这时extract-text-webpack-plugin插件可以帮我们达到想要的效果。需要使用npm的方式加载插件，然后 参见下面的配置，就可以将js中的css文件提取，并以指定的文件名来进行加载。
npm install extract-text-webpack-plugin --save-dev
我发现这个有一个问题，就是他只能把css抽出来，但是sass的样式不能分离出来。
var ExtractTextPlugin = require("extract-text-webpack-plugin");
分离css
loader: ExtractTextPlugin.extract("style-loader", "css-loader") 
分离scss
loader: ExtractTextPlugin.extract('style', "css!sass")
new ExtractTextPlugin("app.css")

**4.3合并配置文件插件**
webpack-config
https://github.com/mdreizin/webpack-config
**4.4创建index.Html页面插件**
html-webpack-plugin
npm install html-webpack-plugin --save-dev

//引用模块
var HtmlWebpackPlugin = require('html-webpack-plugin');
// 配置生成index.html
new HtmlWebpackPlugin({
            title: 'Custom template using Handlebars',
            template: './src/index.html'
        })
**4.5自动打开浏览器插件**
open-browser-webpack-plugin
npm install --save-dev open-browser-webpack-plugin



//引用模块
var OpenBrowserPlugin = require('open-browser-webpack-plugin');
//配置打开浏览器
new OpenBrowserPlugin()


