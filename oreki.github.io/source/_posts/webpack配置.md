---
title: webpack配置
toc: true
mathjx: true
cover: /2020/07/24/webpack配置/head.png
tags:
  - 前端
categories:
  - 前端
  - Vue
abbrlink: 36934
date: 2020-07-24 20:48:06
update:
---
### 建立 webpack+vue-loader 项目
#### 初始化
创建 jtodo 目录，并初始化项目
~~~
$ git init // 初始化 git 仓库
$ npm init // 初始化 npm 项目，目录生成 `package.json` 文件
~~~

安装 webpack 打包工具，vue 解释器
~~~
$ npm i webpack webpack-cli vue-template-compiler css-loader --save-dev
$ npm i vue vue-loader --save-dev
~~~

新建 src 目录，及该目录下的 app.vue 和 index.js 文件
~~~html
// app.vue
<template>
  <div id="app">{{text}}</div>
</template>

<script>
export default {
  data() {
    return {
      text: 'hello world!'
    }
  }
}
</script>

<style>
#app {
  color: red;
}
</style>
~~~

~~~js
// index.js
import Vue from 'vue'
import App from './app.vue'

const root = document.createElement('div')
document.body.appendChild(root)

// 将 app.vue 挂载到 root 元素上
new Vue({
  render: h => h(App)
}).$ mount(root)
~~~

#### 配置项目的打包入口和出口
在根目录新建 webpack.config.js 文件，并配置项目的打包入口和出口
~~~js
// webpack.config.js
const path = require('path')

module.exports = {
  entry: path.join(__dirname,  // 使用绝对路径'src/index.js'),
  ouput: {
    filename: 'bundle.js',
    path: path.join(__dirname, 'dist')
  }
}
~~~

#### 配置 build 脚本并测试
在 package.json 配置 build 脚本
~~~js
// package.json
{
  ...
  "scripts": {
    ...
    "build": "webpack --config webpack.config.js"
  }
}
~~~

此时尝试 $ npm run build ，报错如下
~~~
ERROR in ./src/app.vue 1:0
Module parse failed: Unexpected token (1:0)
You may need an appropriate loader to handle this file type.
> <template>
|   <div>hello</div>
| </template>
...
~~~

### webpack 配置项目加载静态文件和 CSS 预处理器

#### 配置 vue 文件中的 CSS 解释器
~~~js
// webpack.config.js
const { VueLoaderPlugin } = require('vue-loader')

module.exports = {
  ...
  module: {
    rules: [
      {
        test: /.vue$ /,
        loader: 'vue-loader'
      },
      { // vue-loader@15.*之后，还需 css-loader 去解析 vue 中的 style
        test: /\.css$ /,
        use: [
          'css-loader'
        ]
      }
    ]
  },
  plugins: [
    new VueLoaderPlugin()
  ]
}
~~~
运行 $ npm run build ，成功打包项目

#### 配置项目加载静态文件
项目中的一些小静态文件，可以把小静态文件转换成代码，直接写到 JS 文件中，减少 HTTP 请求。

安装解释器
~~~
$ npm i file-loader, url-loader --save-dev
~~~

配置项目加载静态文件
~~~js
// webpack.config.js
module.exports = {
  ...
  module: {
    rules: [
      ...
      {
        test: /\.(gif|jpg|jpeg|png|svg)$ /,
        use: [
          {
            loader: 'url-loader', // 把小静态文件转换成代码，直接写到 JS 文件中，减少 HTTP 请求
            options: {
              limit: 2048, // 单位是 B
              name: '[name]-aaa.[ext]' // 重命名图片
            }
          }
        ]
      }
    ]
  }
}
~~~

接着在目录 src/assets/images/ 下新建 bg.svg 文件测试
~~~
<svg xmlns='http://www.w3.org/2000/svg' width='400' height='400' viewBox='0 0 800 800'><rect fill='#330033' width='800' height='800'/><g fill='none' stroke='#404' stroke-width='1'><path d='M769 229L1037 260.9M927 880L731 737 520 660 309 538 40 599 295 764 126.5 879.5 40 599-197 493 102 382-31 229 126.5 79.5-69-63'/><path d='M-31 229L237 261 390 382 603 493 308.5 537.5 101.5 381.5M370 905L295 764'/><path d='M520 660L578 842 731 737 840 599 603 493 520 660 295 764 309 538 390 382 539 269 769 229 577.5 41.5 370 105 295 -36 126.5 79.5 237 261 102 382 40 599 -69 737 127 880'/><path d='M520-140L578.5 42.5 731-63M603 493L539 269 237 261 370 105M902 382L539 269M390 382L102 382'/><path d='M-222 42L126.5 79.5 370 105 539 269 577.5 41.5 927 80 769 229 902 382 603 493 731 737M295-36L577.5 41.5M578 842L295 764M40-201L127 80M102 382L-261 269'/></g><g fill='#505'><circle cx='769' cy='229' r='5'/><circle cx='539' cy='269' r='5'/><circle cx='603' cy='493' r='5'/><circle cx='731' cy='737' r='5'/><circle cx='520' cy='660' r='5'/><circle cx='309' cy='538' r='5'/><circle cx='295' cy='764' r='5'/><circle cx='40' cy='599' r='5'/><circle cx='102' cy='382' r='5'/><circle cx='127' cy='80' r='5'/><circle cx='370' cy='105' r='5'/><circle cx='578' cy='42' r='5'/><circle cx='237' cy='261' r='5'/><circle cx='390' cy='382' r='5'/></g></svg>
~~~

#### 配置 SCSS 解释器
安装解释器
~~~
$ npm i sass-loader, node-sass, style-loader --save-dev
~~~

配置 SCSS 解释器
~~~js
// webpack.config.js
module.exports = {
  ...
  module: {
    rules: [
      ...
      {
        test: /\.scss$ /,
        use: [
          'style-loader',// 将 JS 字符串生成为 style 节点
          "css-loader", // 将 CSS 转化成 CommonJS 模
          "sass-loader"
        ]
      }
    ]
  }
}
~~~
接着在目录 src/assets/styles/ 下新建 global.scss 文件测试
~~~css
// global.scss
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
 }

body {
  background-image: url('../images/bg.svg');
  background-size: cover;
  background-position: center center;
  font-size: 14px;
  color: #4d4d4d;
  font-weight: 300;
}
~~~

### 配置开发环境
#### 配置 webpack-dev-server
webpack-dev-server 能帮助程序员在开发环境中预览项目

安装
~~~
$ npm i webpack-dev-server cross-env --save-dev
~~~

在 package.json 配置 dev 脚本，并设置环境变量标识环境，同时使得跨平台命令统一（cross-env）

~~~js
// package.json
{
  ...
  "scripts": {
    ...
    "build": "cross-env NODE_ENV=production webpack --config webpack.config.js",
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config webpack.config.js"
  }
}
~~~

在根目录新建 webpack.config.js 文件，并配置项目的开发环境
~~~js
// webpack.config.js
const path = require('path')
const { VueLoaderPlugin } = require('vue-loader')
const webpack = require('webpack') // 引入 webpack

// 启动 build 或 dev 脚本时，会把环境变量存在 process.env 中
const isDev = process.env.NODE_ENV === 'development'

const config = {
  target: 'web', // 编译目标 web 平台
  entry: path.join(__dirname, 'src/index.js'),  // 使用绝对路径
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname, 'dist')
  },
  module: {
    rules: [
      ...
    ]
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: isDev ? '"development"' : '"production"',
      },
    }),
    new VueLoaderPlugin(),
  ]
}

if (isDev) {
  config.devServer = {
    port: 8000,
    host: '0.0.0.0', // 设 IP 为 0.0.0.0 可用同时用 localhost 和内网 IP 访问
    overlay: { // 在网页上报错
      errors: true,
    }
  }
}

module.exports = config
~~~

#### 配置html入口
安装
~~~
$ npm i html-webpack-plugin --save-dev
~~~
预览项目需要有一个 html 文件去包含 index.js ，在 webpack.config.js 添加如下信息
~~~js
// webpack.config.js
...
const HTMLPlugin = require('html-webpack-plugin')

const config = {
  ...
  plugins: [
    ...
    new HTMLPlugin()
  ]
}
~~~

#### 配置热重载和 devtool
前面修改组件代码后，会自动更新整个页面，现在配置修改组件时，仅重新渲染该组件。另外，添加了 devtool 来帮助我们在浏览器调试代码。
~~~js
// webpack.config.js
...
if (isDev) {
  ...
  config.devtool = '#cheap-module-eval-source-map' // 用 devtool 检查代码
  config.plugins.push(
    new webpack.HotModuleReplacementPlugin(), // 模块热重载
    new webpack.NoEmitOnErrorsPlugin(), // 减少不需要信息的展示
  )
}
~~~
