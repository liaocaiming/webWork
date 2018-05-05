#### 一. REPL(read eval print loop) 输入—求值—输出循环;
#### 二. supersivor(插件): 不用重启服务直接更新代码;
        ```
           安装: npm install -g supervisor
           使用: supervisor app.js
        ```
        
#### 三.npm 发布流程
   1. 在发布前，我们还需要获得一个账号用于今后维护自己的包，使用 npm adduser 根据
提示输入用户名、密码、邮箱，等待账号创建完成。完成后可以使用 npm whoami 测验是
否已经取得了账号。
接下来，在 package.json 所在目录下运行 npm publish，稍等片刻就可以完成发布了。
打开浏览器，访问 http://search.npmjs.org/ 就可以找到自己刚刚发布的包了。现在我们可以在
世界的任意一台计算机上使用 npm install byvoidmodule 命令来安装它。


#### 四. util: node模块的一个插件
   1. 方法 util.inherits(子类, 父类), 只能继承原型上的, 构造函数内的不会继承;
      ```
       var util = require('util');

        function A (name) {
            this.name = name;
            this.sayHello = function () {
                console.log('hello')
            }
        }
        
        A.prototype.getName = function () {
            return this.name;
        }
        
        function B (name) {
            this.name = name;
            this.age = 18;
        }
        
        util.inherits(B, A);
        
        
        var b = new B('B');
        
        console.info(b.getName());
        // console.info(b.sayHello());
      ```
    
    2. 方法 util.inspect(Objec, [showHidden], [depth], [colors])将任意对象转化成字符串;     util还提供了util.isArray()、util.isRegExp()、
util.isDate()、util.isError() 四个类型测试工具，以及 util.format()、util.
debug() 等工具
    
#### 五. events模块, node最重要的模块
    
    