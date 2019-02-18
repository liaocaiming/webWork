##### 1. yargs: 获取命令的参数
```js
const argv = require('yargs').argv
const appName = argv.name || 'boms'

```

#### 2. http-proxy-middleware: 用于把请求代理转发到其他服务器的中间件, 解决前端跨域问题: 
文档: https://www.jianshu.com/p/a248b146c55a 

#### 3. fancy-log: 打印日志;
```js
var log = require('fancy-log');
 
log('a message');
// [16:27:02] a message
 
log.error('oh no!');
// [16:27:02] oh no!
```

#### 4. opn: 打开浏览器;
```
const opn = require('opn');
 
// Opens the image in the default image viewer
opn('unicorn.png').then(() => {
    // image viewer closed
});
 
// Opens the url in the default browser
opn('http://sindresorhus.com');
 
// Specify the app to open in
opn('http://sindresorhus.com', {app: 'firefox'});
 
// Specify app arguments
opn('http://sindresorhus.com', {app: ['google chrome', '--incognito']});
```