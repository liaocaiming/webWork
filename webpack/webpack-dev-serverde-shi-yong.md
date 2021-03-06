### 一. webpack.config.js 基本配置
```js
const uglify = require('uglifyjs-webpack-plugin');
const path = require('path');
const htmlWebpackPlugin = require('html-webpack-plugin');
const cleanWebpackPlugin = require('clean-webpack-plugin');
const webpack = require('webpack');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  entry: './src/index.js'
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
        // use: ['style-loader', 'css-loader']
        use: ExtractTextPlugin.extract({
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
    new cleanWebpackPlugin('dist'),
    new webpack.NamedModulesPlugin(),
    new webpack.HotModuleReplacementPlugin(), // 必须的
    new webpack.NoEmitOnErrorsPlugin(),
    new ExtractTextPlugin({
      filename: (getPath) => {
        return getPath('css/[name].css').replace('css/js', 'css');
      },
      allChunks: true
    })
  ]
}
```

### 二.devServer.js
```js
const path = require('path');

module.exports = {
    port: 9000,
    contentBase: path.join(__dirname, "dist"),
    compress: true,
    open: true,
    hot: true,
    host: 'localhost'
}

```

### 三. webpack.dev.js
```js
const Webpack = require('webpack');

const config = require('./webpack.config')

const compiler = Webpack(config);

const path = require('path');

const opn = require('opn');

const WebpackDevServer = require('webpack-dev-server');

const devServerOptions = require('./devServer');

WebpackDevServer.addDevServerEntrypoints(config, devServerOptions)

const server = new WebpackDevServer(compiler, devServerOptions);

server.listen(8080, 'localhost', () => {
   console.log('Starting server on http://localhost:8080');
   opn('http://localhost:8080')
});


```