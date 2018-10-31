---
title: 配置webpack 3
date: 2018-05-31 11:23:30
tags:
---
### 安装
安装之前需要先安装node.js，然后按照官网指示
```
npm install --save-dev webpack@<version>
```
这里我配置的是webpack 3，在本地目录安装。
安装成功后命令行输入
```
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```
## 配置
在安装的跟目录创建一个webpack.config.js
配置如下
```
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
上面配置的意思是将当前目录下的src目录下的index.js 打包为boundle.js输出在dist目录下。这些配置都可以按需更改。
## 使用babel-loader 把 ES6 转译为 ES5
谷歌搜索babel-loader webpack，进入github，按照官网提示配置，注意一点下面的配置是babel 6 和babel-loader 7的配置方法。
```
npm install babel-loader babel-core babel-preset-env webpack
```
把下面代码加入到webpack.config.js
```
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['env']
        }
      }
    }
  ]
}
```
现在往index.js 内写如ES6代码，保存，运行 npx webpack，如果报错缺少module，npm i xxx 安装一下。
## 使用sass-loader 把 SCSS 转译为 CSS
套路跟babel-loader一样，到官网抄代码。
```
npm install sass-loader node-sass webpack --save-dev
```
```
npm install style-loader css-loader --save-dev
```
配置webpack.config.js
```
module.exports = {
	...
    module: {
        rules: [{
            test: /\.scss$/,
            use: [
                "style-loader", // creates style nodes from JS strings
                "css-loader", // translates CSS into CommonJS
                "sass-loader" // compiles Sass to CSS
            ]
        }]
    }
};
```
简单配置OK！
## 使用webpack 模块化
引入js和scss方法
首先在app.js使用import引入js模块，下面的列子引入了两个js模块和scss模块
```
 import x from './modules-1'
 import y from './modules-2'
 import '../css/main.scss'
 x()
 console.log('fuck webpack')
 y()
 let a =1 
 ```
 modules-1.jsd代码
 ```
 function fn(){
	console.log(1)
}
export default fn 
```
因为是demo，代码比较简单主要是有export default fn 这个关键代码。

