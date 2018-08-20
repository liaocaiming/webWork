#### 一.入口文件: webpack.config.js

```
const path = require('path');
const htmlWebpackPlugin = require('html-webpack-plugin');
const cleanWebpackPlugin = require('clean-webpack-plugin');
const webpack = require('webpack');
// const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  // entry: './src/index.js',
  entry: {
    index: ['webpack-hot-middleware/client?path=/__webpack_hmr&timeout=10000&reload=true', './src/index.js']
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
    new ExtractTextPlugin({
      filename: (getPath) => {
        return getPath('css/[name].css').replace('css/js', 'css');
      },
      allChunks: true
    })
  ]
}
```