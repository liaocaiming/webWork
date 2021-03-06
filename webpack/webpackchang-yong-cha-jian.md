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
```js
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

#### 4. lowdb: 作为轻量级的本地存储方式;

```js
const low = require('lowdb')
const FileSync = require('lowdb/adapters/FileSync')
 
const adapter = new FileSync('db.json')
const db = low(adapter)
 
// Set some defaults
db.defaults({ posts: [], user: {} })
  .write()
 
// Add a post
db.get('posts')
  .push({ id: 1, title: 'lowdb is awesome'})
  .write()
 
// Set a user using Lodash shorthand syntax
db.set('user.name', 'typicode')
  .write()
  
```

```json
{
  "posts": [
    { "id": 1, "title": "lowdb is awesome"}
  ],
  "user": {
    "name": "typicode"
  }
}
```

#### 5. detect-port: 检测端口是否被占用;

```js
const detect = require('detect-port');
 
/**
 * callback usage
 */
 
detect(port, (err, _port) => {
  if (err) {
    console.log(err);
  }
 
  if (port == _port) {
    console.log(`port: ${port} was not occupied`);
  } else {
    console.log(`port: ${port} was occupied, try port: ${_port}`);
  }
});

detect(port)
  .then(_port => {
    if (port == _port) {
      console.log(`port: ${port} was not occupied`);
    } else {
      console.log(`port: ${port} was occupied, try port: ${_port}`);
    }
  })
  .catch(err => {
    console.log(err);
  });
 
```

#### 6. cheerio: 是jquery核心功能的一个快速灵活而又简洁的实现，主要是为了用在服务器端需要对DOM进行操作的地方;

文档: https://www.jianshu.com/p/629a81b4e013

#### 7. is-absolute-url: 判断是否绝对路径;

#### 8. htmlparser2: 一个快速和宽容的HTML/XML/RSS解析器，解析器可以出来流，并且提供了一个回调接口。

#### 9. node-ssh: 文件上传到服务器;

#### 10. ora: 主要用来实现node.js命令行环境的loading效果，和显示各种状态的图标等;

#### 11. json5: 处理json文件, 是json文件可以写注释等;