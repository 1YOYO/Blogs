---
title: gulp搭建
---

  今天是我写hexo的开篇文章，主要是想初探一下hexo的流程，以及复习复习之前学习的基础东西，也为了以后自己方便观看和分享学习成果。初学者，基础知识不是太官方。。见谅。。直奔主题。
  今天主要想说的是gulp的搭建，但是在gulp搭建前，需要搭配好相关环境，主要内容为：

> * Node简介及Node的环境搭建
> * gulp的搭建

------
>## 一、Node简介及搭建

### **1.1Node.js是什么？**
Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

注意：Node.js 的包管理器 npm，是全球最大的开源库生态系统。node中只能运行ECMAScript，无法使用 BOM 和 DOM，所以说到底，他就是一个js运行环境。

### ***1.2Node环境的搭建***
Node版本发展很快，版本前后差异性大，老系统用新版本node跑不过，	全局安装的第三方组件和node版本相关造成全局版本混乱。然而nvm是解决这一问题的利器。
nvm是node版本管理工具。它的主要特点是：

 1. 可安装多版本的node。
 2. 灵活切换当前的node版本。
 3. 全局安装第三方组件到对应版本的node中。

#### ***1.2.1使用nvm安装Node.js***
nvm 是 Mac 下的 node 管理工具,如果是需要管理 Windows 下的 node，官方推荐是使用 nvmw 或 nvm-windows
**下载 nvm 包 地址：https://github.com/coreybutler/nvm-windows/releases**
下载安装nvm这里就不在赘述了(需要注意最好不要将其安装在中文目录下)。
安装 nvm 之前最好先删除下已安装的 node 和全局 node 模块
#### ***1.2.2配置相关的环境变量***
1. 先使用管理员权限运行install.cmd，
2. 根目录生成一个settings.txt的文本文件,将其剪切到nvm目录下。
然后我们把他修改成这样：

    root: D:\dev\nvm      //这里表示的是当前文件夹路径
    path: D:\dev\nodejs   //这里表示配置后要要生成的目录
    arch: 64              //这里为自己电脑的机器位数
    proxy: none
3. 先***配置nvm的环境变量***，让其能够在cmd中使用其命令
我们还会发现，在Path中也会自动添加上D:\dev\nvm;或者是D:\dev\nodejs，如果有的话，把他们删掉，没有的话更好。
->先添加：NVM_HOME的变量值为：D:\dev\nvm
->再在Path的最前面输入： ;%NVM_HOME%;
->打开一个cmd窗口输入命令：nvm v,如果我们会看到当前nvm的版本信息。然后我们可以安装nodejs了
4. ***安装nodejs**并**配置相关环境***
配置node环境:先添加：NVM_HOME的变量值为：D:\dev\nodejs,再在Path的最前面输入： ;%NVM_SYMLINK%;
继续输入命令：nvm install latest 我们会看到正在下载的提示，下载完成后 会让你use那个最新的node版本。
如果你是第一次下载，在use之前，D:\dev目录下是没有nodejs这个文件夹的，在输入比如： nvm use 5.11.0之后，你会发现，D:\dev目录下多了一个nodejs文件夹，这个文件夹不是单纯的文件夹，它是一个快捷方式，指向了 D:\dev\nvm 里的 v5.11.0 文件夹。
同样的咱们可以下载其他版本的nodejs，这样通过命令:nvm use 版本号 比如：nvm use 5.11.0就可以轻松实现版本切换了。
**备注**： 如果你的电脑系统是32位的，那么在下载nodejs版本的时候，一定要指明 32 如： nvm install 5.11.0 32这样在32位的电脑系统中，才可以使用，默认是64位的。

5. ***安装npm及相关指令***
1. npm 是 nodejs 的包管理和分发工具。它可以让javascript 开发者能够更加轻松的共享代码和共用代码片段，并且通过 npm 管理你分享的代码也很方便快捷和简单。
**备注**：在每个版本的nodejs中，都会自带npm，为了统一起见，我们安装一个全局的npm工具，这个操作很有必要，因为我们需要安装一些全局的其他包,不会因为切换node版本造成原来下载过的包不可用。
2. 首先我们进入命令模式,输入 npm config set prefix "D:\dev\nvm\npm" 回车，这是在配置npm的全局安装路径,然后在用户文件夹下会生成一个。npmrc的文件,用记事本打开后可以看到如下内容：
                    prefix=D:\dev\nvm\npm
3. 然后继续在命令中输入： npm install npm -g 回车后会发现正在下载npm包,在D:\dev\nvm\npm目录中可以看到下载中的文件,以后我们只要用npm安装包的时候加上 -g 就可以把包安装在我们刚刚配置的全局路径下了。
4. 我们为这个npm配置环境变量： 变量名为：NPM_HOME，变量值为 ：D:\dev\nvm\npm在Path的最前面添加;%NPM_HOME%，注意了，这个一定要添加在 %NVM_SYMLINK%之前，所以我们直接把它放到Path的最前面.最后我们新打开一个命令窗口，输入npm -v ,此时我们使用的就是我们统一下载的npm包了。
5. 相关指令：
初始化：npm init
安装： npm install PackageName --save-dev
卸载： npm uninstall PackageName --save-dev


-------

>## 二、gulp

###***2.1gulp是什么***
gulp是前端开发过程中一种基于流的代码构建工具，是自动化项目的构建利器。它能自动化地完成 javascript、coffee、sass、less、html/image、css 等文件的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。


### ***2.2gulp安装及搭建工作流程***

 1. 创建一个工程文件夹，打开cmd进入该文件夹目录，输入npm init初始化，之后会形成一个packa.json文件用来管理一些下载的依赖包。
 2. 下载gulp。npm installgulp --save-dev.
 3. 手动建立一个gulpfiles.js,和开发用到的文件夹：*.css、*.js、*.html、*.img、*.fonts等。
 4. 下面所有的代码都需要写在gulpfiles.js文件夹里，主要的功能是压缩sass,js,html，监听等
 5. scss-->css并压缩
    * 先下载gulp-sass用来将**sass转化成css**：npm install gulp-sass --save-dev
    * 下载gulp-cssnano用来将**css文件压缩**：npm install gulp-cssnano --save-dev
    * 然后再gulpfiles.js中写一下代码，后面有注释。
```python
var gulp = require('gulp');         //下载gulp后需要引入gulp包
var sass = require('gulp-sass');    //引入gulp-sass包
var cssnano = reguire('gulp-cssnano')  //引包

gulp.task('style',function(){       //task()是gulp里的一个方法
    gulp.src(css/*.scss)            //src()是写scss格式的路径
    .pipe(sass())               //然后执行sass()函数进行压缩
    .pipe(cssnano())
    .pipe(gulp.dest('dest("dist/css/")'))//dest()将压缩的css放到要发布的目录
})
```
然后在cmd中执行: gulp style就可以看到目录中生成一个dest文件夹里面有*.css,而且是压缩过的
    6. 将js代码压缩并发送到要发布的文件：
   
* 将多个js合并的包gulp-concat:npm install --save-dev gulp-concat
* 将js压缩的包：gulp-uglify:npm install --save-dev gulp-uglify
* 然后再上面的基础上再添加如下代码：
```python
var concat = require('gulp-concat');
var uglify = reguire('gulp-uglify')
gulp.task('script',function(){
    gulp.src('js/*.js')
    .pipe(concat())
    .pipe(uglify())
    .pipe(gulp.dest('dist/js'))
})
```
* 在cmd中执行：gulp script就可以看到生成的js的文件
    7.对html进行压缩
* 下载htmlmin:npm install --save-dev htmlmin
```python
var htmlmin = require('gulp-htmlmin');
gulp.task('html',function(){
    var options = {
        collapseWhitespace:true,
        collapseBooleanAttributes:true,
        removeComments:true,
        removeEmptyAttributes:true,
        removeStyleLinkTypeAttributes:true,
    };
    gulp.src('*.js')
    .pipe(htmlmin(options))
    .pipe(gulp.dest('dist/'))
})
```
* 然后再cmd中执行：gulp html
**备注：**
1.collapseWhitespace:清除空格，压缩html
2.collapseBooleanAttributes:省略布尔属性的值
3.removeComments:清除html中注释的部分
4.removeEmptyAttributes:清除所有的空属性
还没写上的参数：
5.removeSciptTypeAttributes:清除所有script标签中的type="text/javascript"属性。
6.removeStyleLinkTypeAttributes:清楚所有Link标签上的type属性。
7.minifyJS:压缩html中的javascript代码。
8.minifyCSS:压缩html中的css代码。

        8. 将图片拷入到dist下：
```pathy
gulp.task('img',function(){
    pipe.src('img/*.*')
    .pipe(gulp.dest("dist/img"))
})
```
        9. 将fonts移入dist文件，同理，不在赘述。
        10. 构建服务器，这里用的是browser-syncbrowser-sync,下载npm install --save-dev browser-sync 
```pathy
var = require ('browser-sync');
gulp.task('serve',function(){
    browserSync.init({
        server:{baseDir:"dist/"}
    })
})
``` 
可以在在cmd中执行以下，gulp serve然后再自己本的浏览器输入本地地址。
        11.设置监听
```pathy
gulp.watch("*.html",["html"]);
gulp.watch("css/*.scss",["style"]);
gulp.watch("js/*js",["script"]);
gulp.watch("img/*.*",["img"]);
```    
12. 刷新前需要重载：
```pathy
var reload = browserSync.reload;

```   
13. 在每一个task中都需要添加这一句：
```pathy
    .pipe(reload({steam:true}))
```
呼啦啦,让我大喘气一下，写了好久啊，凭记忆和各种资料，终于完结。。