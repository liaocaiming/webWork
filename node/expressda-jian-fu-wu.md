### nodejs搭建服务器
```
var express = require('express');
var app = express();
var fs = require('fs');
var path = require('path');

app.all('*', function (req, res, next) { // 解决浏览器不同端口的跨域问题
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Content-Length, Authorization, Accept, X-Requested-With , yourHeaderFeild');
    res.header('Access-Control-Allow-Methods', 'PUT, POST, GET, DELETE, OPTIONS');
    if (req.method == 'OPTIONS') {
        res.send(200); /// 让options请求快速返回/
    }
    else {
        next();
    }
})

function resolve(dir) {
    return path.join(__dirname, dir)
}

function getFilePath(rootPath, app) {
    var rootUrl = resolve(rootPath);
    var files = fs.readdirSync(rootUrl);
    files.forEach(function (file) {
        let url = `${rootUrl}/${file}`;
        fs.stat(url, function (err, stat) {
            if (err) {
                return;
            }
            if (stat.isFile()) {
                getFilePath(url, app)
            } else {
                fs.readdirSync(url).forEach(function (item) {
                    var getData = require(`${url}/${item}`);
                    getData(app);
                })
            }
        })
    })
}

getFilePath('./data', app);

app.listen(5000, function () {
    console.log('5000 listening...');
});

```
### 另外一个文件
```
function getUserInfo(app) {
  // 获取用户信息
  app.get('/wxapp/user/wxUserinfo', (req, res) => {
    res.send({
      nickname: '飘零',
      avatar: 'https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1486083553,2709114243&fm=27&gp=0.jpg',
      token: '4321reffdsafdsareqreweruowqire'
    })
  })

}
module.exports = getUserInfo

```
