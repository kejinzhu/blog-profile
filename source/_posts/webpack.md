---
title: webpack基础
copyright: true
date: 2018-08-17 10:07:55
categories: 前端
tags:
     - webpack
     - 前端
---

# 什么是webpack?

WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

<!-- more -->

> 构建就是把源代码转换成发布到线上的可执行 JavaScrip、CSS、HTML 代码，包括如下内容。

- **代码转换:** TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等。
- **文件优化:** 压缩 JavaScript、CSS、HTML 代码，压缩合并图片等。
- **代码分割:** 提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载。
- **模块合并:** 在采用模块化的项目里会有很多个模块和文件，需要构建功能把模块分类合并成一个文件。
- **自动刷新:** 监听本地源代码的变化，自动重新构建、刷新浏览器。
- **代码校验:** 在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过。
- **自动发布:** 更新完代码后，自动构建出线上发布代码并传输给发布系统。

构建其实是工程化、自动化思想在前端开发中的体现，把一系列流程用代码去实现，让代码自动化地执行这一系列复杂的流程。 构建给前端开发注入了更大的活力，解放了我们的生产力。

# 初始化项目

```javascript
mkdir webpack-public
cd webpack-public
npm init -y
```

以上三条命令分别是：创建文件夹并命名为webpack-public，将文件夹添加进路径中，初始化npm。

> 注意：在创建文件夹的时候，名字不能是webpack，否在在安装webpack的时候会报错。

错误信息：

> npm ERR! code ENOSELF
npm ERR! Refusing to install package with name "webpack" under a packagenpm ERR! also called "webpack". Did you name your project the same
npm ERR! as the dependency you're installing?
npm ERR!
npm ERR! For more information, see:
npm ERR!     <https://docs.npmjs.com/cli/install#limitations-of-npms-install-algorithm>

# webpack核心知识
## webpack的核心概念

- **Entry:**入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入。
- **Module：**模块，在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。
- **Chunk：**代码块，一个 Chunk 由多个模块组合而成，用于代码合并与分割。
- **Loader：**模块转换器，用于把模块原内容按照需求转换成新内容。
- **Plugin：**扩展插件，在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情。
- **Output：**输出结果，在 Webpack 经过一系列处理并得出最终想要的代码后输出结果。

> Webpack 启动后会从Entry里配置的Module开始递归解析 Entry 依赖的所有 Module。 每找到一个 Module， 就会根据配置的Loader去找出对应的转换规则，对 Module 进行转换后，再解析出当前 Module 依赖的 Module。 这些模块会以 Entry 为单位进行分组，一个 Entry 和其所有依赖的 Module 被分到一个组也就是一个 Chunk。最后 Webpack 会把所有 Chunk 转换成文件输出。 在整个流程中 Webpack 会在恰当的时机执行 Plugin 里定义的逻辑。

## 配置webpack

> npm install webpack webpack-cli -D

在开发环境中安装webpack webpack-cli

## 创建src目录

> mkdir src

## 创建dist目录

> mkdir dist

## 基本配置文件
### 创建dist目录下的index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="root"></div>
    <script src="bundle.js"></script>
</body>

</html>
```

### 配置文件webpack.config.js

- **entry：**配置入口文件的地址
- **output：**配置出口文件的地址
- **module：**配置模块,主要用来配置不同文件的加载器
- **plugins：**配置插件
- **devServer：**配置开发服务器

```javascript
const path=require('path');
module.exports={
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    },
    module: {},
    plugins: [],
    devServer: {}
}
```

## 配置开发服务器

> npm install webpack-dev-server -D

尽量不要简写install为i,有可能会报这个错误

```javascript
npm ERR! code EINVALIDTAGNAME
npm ERR! Invalid tag name "–D": Tags may not have any characters that e
ncodeURIComponent encodes.
npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\User\AppData\Roaming\npm-cache\_logs\2018-08-17T03_07_44_727Z-debug.log
```

在webpack.config.js文件中，找到devServer添加如下内容

```javascript
devServer: {
        contentBase: path.resolve(__dirname, 'dist'),
        host: 'localhost',
        compress: true,
        port: 8080
    }
```

 - **contentBase:**配置开发服务运行时的文件根目录
 - **host：**开发服务器监听的主机地址
 - **compress:** 开发服务器是否启动gzip等压缩

 - **port：**开发服务器监听的端口
在pack.json中找到scripts添加如下内容

```javascript
"scripts": {
        "build": "webpack --mode development",
        "dev": "webpack-dev-server --open --mode development "
    }
```

## 支持加载CSS文件
### 什么是Loader

通过使用不同的loader，Webpack可以把不同的文件都转成js文件。比如CSS、ES6/7、JSX等

 - **test：**匹配处理文件的扩展名的正则表达式
 - **use**：loader名称，就是你要使用模块的名称
 - **include/exclude:**手动指定必须处理的文件夹或屏蔽不需要处理的文件夹
 - **query：**为loaders提供额外的设置选项

### loader的三种写法
#### loader

加载CSS文件，CSS文件有可能在node_modules里，比如bootstrap、antd

```javascript
module: {
        rules: [
            {
                test: /\.css/,                 
                loader:['style-loader','css-loader']
            }
        ]
    }
```

#### use

```javascript
module: {
        rules: [
            {
                test: /\.css/,               
                use:['style-loader','css-loader']
            }
        ]
    },
```

#### use+loader

```javascript
module: {
        rules: [
            {
                test: /\.css/,
                include: path.resolve(__dirname,'src'),
                exclude: /node_modules/,
                use: [{
                    loader: 'style-loader',
                    options: {
                        insertAt:'top'
                    }
                },'css-loader']
            }
        ]
    }
```

## 插件

- 在 webpack的构建流程中，plugin用于处理更多其他的一些构建任务
- 模块代码转换的工作由 loader 来处理
- 除此之外的其他任何工作都可以交由 plugin 来完成

### 自动产出html

#### 安装插件

自动产出HTML文件，并在里面引入产出后的资源

> npm install html-webpack-plugin -D

ps:如果报一下错误

```javascript
npm ERR! code EAI_AGAIN
npm ERR! errno EAI_AGAIN
npm ERR! request to https://registry.npmjs.org/html-webpack-plugin failed, reason: getaddrinfo EAI_AGAIN registry.npmjs.org registry.npmjs.org:443
```

则可以通过以下方法进行解决：切换淘宝镜像

 - 通过config命令

>  npm config set registry https://registry.npm.taobao.org 
 npm info underscore （如果上面配置正确这个命令会有字符串response）
 
 - 命令行指定
 
> npm --registry https://registry.npm.taobao.org info underscore 

执行完成之后，再执行以上命令就可以了，亲测有效

#### 配置webpack.config.js下的plugins

```javascript
plugins: [
        new HtmlWebpackPlugin({
        minify: {
            removeAttributeQuotes:true
        },
        hash: true,
        template: './src/index.html',
        filename:'index.html'
})]
```

- **minify**是对html文件进行压缩，removeAttrubuteQuotes是去掉属性的双引号
- **hash** 引入产出资源的时候加上查询参数，值为哈希避免缓存
- **template** 模版路径

## 支持图片
### 手动添加图片

> npm install file-loader url-loader -D

- **file-loader** 解决CSS等文件中的引入图片路径问题
- **url-loader** 当图片小于limit的时候会把图片BASE64编码，大于limit参数的时候还是使用file-loader 进行拷贝

### JS中引入图片
#### JS

```javascript
let logo=require('./images/logo.png');
let img=new Image();
img.src=logo;
document.body.appendChild(img);
```

#### 配置webpack.config.js

```javascript
{
  test:/\.(jpg|png|bmp|gif|svg|ttf|woff|woff2|eot)/,
    use:[
    {
       loader:'url-loader',
       options:{limit:4096}
    }
  ]
}
```
### 在CSS中引入图片
#### CSS

```css
.logo{
    width:355px;
    height:133px;
    background-image: url(./images/logo.png);
    background-size: cover;
}
```

#### HTML

```html
<div class="logo"></div>
```

## 分离CSS
因为CSS的下载和JS可以并行，当一个HTML文件很大的时候，我们可以把CSS单独提取出来加载

- mini-css-extract-plugin
- filename 打包入口文件
- chunkFilename用来打包import('module')方法中引入的模块

### 安装依赖模块

> npm install --save-dev mini-css-extract-plugin

### 配置webpack.config.js

```javascript
plugins: [
       //参数类似于webpackOptions.output
        new MiniCssExtractPlugin({
            filename: '[name].css',
            chunkFilename:'[id].css'
        }),

{
        test: /\.css/,
        include: path.resolve(__dirname,'src'),
        exclude: /node_modules/,
        use: [{
            loader: MiniCssExtractPlugin.loader
        },'css-loader']
}
```

### 压缩CSS和JS

- [uglifyjs-webpack-plugin][1]
- [optimize-css-assets-webpack-plugin][2]

安装依赖

> npm install uglifyjs-webpack-plugin -D
npm install optimize-css-assets-webpack-plugin -D

```javascript
const UglifyJSplugin=require('uglifyjs-webpack-plugin');
+ const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');

optimization: {
        minimizer: [            
          new UglifyJSplugin({
                cache: true,//启用缓存
                parallel: true,// 使用多进程运行改进编译速度               sourceMap:true//生成sourceMap映射文件
          }),
          new OptimizeCssAssetsWebpackPlugin({})
      ]    
}
```

### CSS和image存放单独目录

- **outputPath** 输出路径
- **publicPath**指定的是构建后在html里的路径

```javascript
output: {
        path: path.resolve(__dirname,'dist'),
        filename: 'bundle.js',
        publicPath:'/'
    },
{
  test:/\.(jpg|jpeg|png|bmp|gif|svg|ttf|woff|woff2|eot)/,
  use:[
        {
          loader:'url-loader',
          options:{
              limit: 4096,
              outputPath: 'images',
              publicPath:'/images'
          }
        }
     ]
}

plugins: [
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
            chunkFilename:'css/[id].css'
        }),
```

### 在HTML中使用图片

> npm install html-withimg-loader -D

#### index.html文件中

> <img src="./images/logo.png"/>

#### 在webpack.config.js文件中添加如下配置

```javascript
{
    test: /\.(html|htm)$/,
    use: 'html-withimg-loader'
}
```
## 编译less和sass
### 安装less

> npm install less less-loader -D
npm install node-saas sass-loader -D

如果在安装sass中出现如下错误

```javascript
npm ERR! code E404
npm ERR! 404 Not Found: node-saas@latest
```

可以尝试一下操作
我切换到了cnpm上安装就可以了，我也不明白什么原因

### 写样式
#### less

```less
@color:orange;
.less-container{
    color:@color;
}
```

#### sass

```sass
$color:green;
.sass-container{
    color:green;
}
```

#### wepack.config.js

```javascript
{
        test: /\.less/,
        include: path.resolve(__dirname,'src'),
        exclude: /node_modules/,
        use: [{
            loader: MiniCssExtractPlugin.loader,
        },'css-loader','less-loader']
    },
    {
        test: /\.scss/,
        include: path.resolve(__dirname,'src'),
        exclude: /node_modules/,
        use: [{
            loader: MiniCssExtractPlugin.loader,
        },'css-loader','sass-loader']
},
```

## 处理CSS3属性前缀

为了浏览器的兼容性，有时候我们必须要加入浏览器内核的前缀。目前主流浏览器的内核如下

- **Trident内核：**主要代表为IE浏览器, 前缀为-ms
- **Gecko内核：**主要代表为Firefox, 前缀为-moz
- **Presto内核：**主要代表为Opera, 前缀为-o
- **Webkit内核：**产要代表为Chrome和Safari, 前缀为-webkit

安装依赖

> npm install postcss-loader autoprefixer -D


[postcss-loader][3] 
### index.css

```css
::placeholder {
    color: red;
}
```

### 配置webpack.config.css

```css
module.exports={
    plugins:[require('autoprefixer')]
}
```

### 配置webpack.config.js

```javascript
{
   test:/\.css$/,
   use:[MiniCssExtractPlugin.loader,'css-loader','postcss-loader'],
   include:path.join(__dirname,'./src'),
   exclude:/node_modules/
}
```

## 转义ES6/ES7/JSX
Babel其实死一个编译JavaScript的平台，可以把ES6/ES7,React的JSX转义为ES5

### 安装依赖
> npm install babel-core babel-loader babel-preset-env babel-preset-stage-0 babel-preset-react babel-plugin-transform-decorators-legacy -D

报错:
```javascript
npm ERR! code ENOENT
npm ERR! errno -4058
npm ERR! syscall access
npm ERR! enoent ENOENT: no such file or directory, access 
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent
```

尝试解决办法，切换cnpm安装，我成功了

### decorator

```javascript
//Option+Shift+A
function readonly(target,key,discriptor) {
    discriptor.writable=false;
}

class Person{
    @readonly PI=3.14;
}
let p1=new Person();
p1.PI=3.15;
console.log(p1)
```

#### 配置jsconfig.json

```javascript
{
    "compilerOptions": {
        "experimentalDecorators": true
    }
}
```

### webpack.config.js

```javascript
{
    test: /\.jsx?$/,
    use: {
        loader: 'babel-loader',
        options: {
            presets: ["env","stage-0","react"],
            plugins:["transform-decorators-legacy"]
        }
    },
    include: path.join(__dirname,'src'),
    exclude:/node_modules/
}
```

## 如何调试打包后的代码

webpack通过配置可以自动给我们source maps文件，map文件是一种对应编译文件和源文件的方法

- source-map 把映射文件生成到单独的文件，最完整最慢
- cheap-module-source-map 在一个单独的文件中产生一个不带列映射的Map
- eval-source-map使用eval打包源文件模块,在同一个文件中生成完整sourcemap
- cheap-module-eval-source-map sourcemap和打包后的JS同行显示，没有映射列

> devtool:'eval-source-map'
## 打包第三方类库


  [1]: https://github.com/webpack-contrib/uglifyjs-webpack-plugin
  [2]: https://github.com/NMFR/optimize-css-assets-webpack-plugin
  [3]: https://github.com/postcss/postcss-loader