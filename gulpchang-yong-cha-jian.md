#### 1.gulp-clean: 删除文件;
#### 2. fancy-log: 打印日志;
```js
var log = require('fancy-log');
 
log('a message');
// [16:27:02] a message
 
log.error('oh no!');
// [16:27:02] oh no!
```

#### 3. Chalk: 给打印的信息添加颜色;
```js
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
```

#### 4. gulp-tap: 可以在gulp的pipeg管道中处理文件;
```js
gulp.src("src/**/*.{coffee,js}")
    .pipe(tap(function(file, t) {
        if (path.extname(file.path) === '.coffee') {
            return t.through(coffee, []);
        }
    }))
    .pipe(gulp.dest('build'));
```
#### 5. gulp-zip: 文件夹压缩;
```js
const gulp = require('gulp');
const zip = require('gulp-zip');
 
gulp.task('default', () =>
    gulp.src('src/*')
        .pipe(zip('archive.zip'))
        .pipe(gulp.dest('dist'))
);
```

