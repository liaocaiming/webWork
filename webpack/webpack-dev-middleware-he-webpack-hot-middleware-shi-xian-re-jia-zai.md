#### 一.入口文件: webpack.config.js

```js
const path = require('path');
const htmlWebpackPlugin = require('html-webpack-plugin');
const cleanWebpackPlugin = require('clean-webpack-plugin');
const webpack = require('webpack');
// const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  entry: {
    index: ['webpack-hot-middleware/client?reload=true', './src/index.js'] // 需要reload的js文件
  },
   
  mode: 'development',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[hash]fezs.js',
    publicPath: path.resolve(__dirname, 'dist')
  },
  devtool: "source-map",
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
        // use: ExtractTextPlugin.extract({ // 使用这里时, css不能hotReload
          fallback: "style-loader",
          use: "css-loader",
          allChunks:true
        })
      }
    ]
  },
  plugins:[
    new htmlWebpackPlugin({
      filename: 'index.html',
      template: './tpl/index.html',
      inject: true
    }),
    new webpack.HotModuleReplacementPlugin(), // 必须的
    // new ExtractTextPlugin({
      filename: (getPath) => {
        return getPath('css/[name].css').replace('css/js', 'css');
      },
      allChunks: true
    })
  ]
}
```

#### 二.webpack.dev.js 文件
```
const Webpack = require('webpack');

const config = require('./webpack.config')

const compiler = Webpack(config);

const path = require('path');

const opn = require('opn');

const express = require('express');

const app = express();

const devMiddleware = require('webpack-dev-middleware')(compiler, {
  stats: {
    colors: true,
    chunks: false
  }
})

const hotMiddleware = require('webpack-hot-middleware')(compiler)

app.use(devMiddleware);

app.use(hotMiddleware)

app.listen(9000, 'localhost', () => {
  console.log('Starting server on http://localhost:8080');
   opn('http://localhost:9000') // 打开浏览器
})
```