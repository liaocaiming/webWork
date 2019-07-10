#### 1.gulp-clean: 删除文件;
#### 2. fancy-log: 打印日志;
```
var log = require('fancy-log');
 
log('a message');
// [16:27:02] a message
 
log.error('oh no!');
// [16:27:02] oh no!
```js

#### 3. Chalk: 给打印的信息添加颜色;
```
const chalk = require('chalk');
const log = console.log;
 
// Combine styled and normal strings
log(chalk.blue('Hello') + ' World' + chalk.red('!'));
 
// Compose multiple styles using the chainable API
log(chalk.blue.bgRed.bold('Hello world!'));
 
// Pass in multiple arguments
log(chalk.blue('Hello', 'World!', 'Foo', 'bar', 'biz', 'baz'));
 
// Nest styles
log(chalk.red('Hello', chalk.underline.bgBlue('world') + '!'));
```js