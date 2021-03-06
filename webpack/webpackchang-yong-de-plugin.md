#### 1. html-webpack-plugin: 
**作用:**
为html文件中引入的外部资源如script、link动态添加每次compile后的hash，防止引用缓存的外部文件问题

可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置N个html-webpack-plugin可以生成N个页面入口

```

```

---

#### 2.mini-css-extract-plugin:
**作用:**
将CSS提取为独立的文件的插件，对每个包含css的js文件都会创建一个CSS文件，支持按需加载css和sourceMap
只能用在webpack4中，对比另一个插件 extract-text-webpack-plugin优点:

异步加载
不重复编译，性能更好
更容易使用
只针对CSS
目前缺失功能，HMR。

#### 3. copy-webpack-plugin: 拷贝文件到目标文件夹;
```js
const CopyPlugin = require('copy-webpack-plugin');
 
module.exports = {
  plugins: [
    new CopyPlugin([
      { from: 'source', to: 'dest' },
      { from: 'other', to: 'public' },
    ]),
  ],
};
```

#### 4. webpack-bundle-analyzer: 依赖分析;
```js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
 
module.exports = {
  plugins: [
    new BundleAnalyzerPlugin()
  ]
}
```
#### 5. fork-ts-checker-webpack-plugin: ts校验插件;
```js
new ForkTsCheckerWebpackPlugin({
  tslint: true,
  vue: true
})
```

#### 6. webpack-merge: 对象合并;



#### 9. 