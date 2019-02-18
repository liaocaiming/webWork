##### 1. yargs: 获取命令的参数
```js
const argv = require('yargs').argv
const appName = argv.name || 'boms'

```

#### 2. http-proxy-middleware: 用于把请求代理转发到其他服务器的中间件, 解决前端跨域问题