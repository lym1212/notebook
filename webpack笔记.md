## 概念

- 静态模块打包工具
- 前端项目 **工程化** 的具体解决方案
- 前端模块化开发支持，代码压缩混淆，浏览器端 javascript 兼容性，性能优化

## 初始化项目

```shell
npm init -y
```

## webpck.config.js 配置

```javascript
const path = require('path')

module.exports = {
    // 指定构建模式 production 和 development
    // development 打包快，体积大
    // production 打包慢，体积小
    mode: 'development',
    // webpack 4.x & 5.x
	// 默认打包入口文件 src -> index.js
	// 默认输出文件  dist -> main.js
    // 指定打包入口文件
    entry: path.join(__dirname, './src/xxx.js'),
    output: {
        // 输出文件的存放目录
        path: path.join(__dirname, './dist'),
        // 输出文件的文件名
        filename: 'js/xxxx.js'
    },
    devServer: {
        // 打包后自动打开浏览器
        open: true,
        // 主机地址
        host: '127.0.0.1',
        // http 协议中，端口号 80，会被省略
        port: 80
    },
    module: {
        // 匹配规则
        rules: [
            { 
                test: /\.css$/, // 匹配的文件类型
                use: ['style-loader', 'css-loader']  // 要调用的 loader，从后往前调用 
            }
        ]
    }
}
```

## package.json 配置

```javascript
scripts: {
    "dev": "webpack",
    "build": "webpack --mode production"    // 打包发布，代码压缩和性能优化
}
// npm run dev 执行打包
// 生成一个 dist 文件夹，包含 main.js
```

## 插件

#### webpack-dev-server

```shell
npm install webpack-dev-server --save-dev
```

- 类似于 nodemon，启动一个实时打包的 http 服务器，输出文件会生成在内存中（引用在根目录中的的输出文件（磁盘上看不到）而不是 dist 中的）

  ```javascript
  // package.json 中配置
  scripts: {
      "dev": "webpack serve"
  }
  ```

#### html-webpack-plugin

```shell
npm i --save-dev html-webpack-plugin
```


- 生成一个 html 文件，自动引用打包的 js 文件

  ```javascript
  // webpack.config.js 中配置
  const HtmlWebpackPlugin = require('html-webpack-plugin')
  // 创建实例
  const HtmlWebpackPlugin = new HtmlWebpackPlugin({
      // 原文件
      template: './src/index.html',
      // 生成的文件，在内年中，默认是 dist -> index.html
      filename: './index.html'
  })
  // 挂载
  module.exports = {
      plugins: [HtmlWebpackPlugin]
  }
  ```

#### clean-webpack-plugin

```shell
npm install --save-dev clean-webpack-plugin
```

- 每次打包自动删除 dist 目录

  ```javascript
  // 解构赋值
  const { CleanWebpackPlugin } = require('clean-webpack-plugin')
  
  module.exports = {
      plugins: [new CleanWebpackPlugin()]
  }
  ```

## loader 加载器

- 协助 webpack 打包处理特定的文件模块
- webpack 默认只能打包 js 文件，其他后缀名的文件要使用 loader 加载器才能打包

#### style-loader & css-loader

```javascript
{ test: /\.css$/, use: ['style-loader', 'css-loader'] }
```

#### less & less-loader

```javascript
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
```

#### file-loader & url-loader

```javascript
// 小于limit，会被转换成 base64 格式
{ test: /\.(png|jpg|gif)$/i, use: 'url-loader?limit=8192&outputPath=images'}
```

#### babel-loader

```shell
npm install -D babel-loader @babel/core @babel/preset-env webpack
```

```javascript
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
```

- babel.config.js

## Source Map

- 存储着压缩后的代码对应的压缩前的位置信息

- 开发环境下默认生成的 Source Map 记录的是生成后的代码位置

- 保持一致的配置（开发环境下，方便性）

  ```javascript
  // webpack.config.js
  module.exports = {
      devtool: 'eval-source-map'
  }
  ```

- 发布时省略 devtool 选项，生成的文件中不包含 Source Map，防止源代码泄露

- 只定位报错行数，不暴露源码（生产环境下，安全性）

  ```javascript
  devtool: 'nosources-source-map'
  ```

## 别名配置

```javascript
// webpack.config.js
resolve: {
    alias: {
        '@': path.join(__dirname, '/src/')
    }
}
```

## npm 简写

```shell
-D -> --save-dev
-S -> --save
i  -> install
```

