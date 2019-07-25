```js
const fs = require('fs');
const path = require('path');
function iterateFiles (rootDir, callBack) {
  const fileList = [];
  let remainTimes = 1;

  return new Promise((resolve, reject) => {
    function travel (dir, callBack, finish) {
      fs.readdir(dir, (err, files) => {
        const len = files.length;
        remainTimes--
        if (err)  {
          reject()
        };
        remainTimes += len;

        (function next(i) {
          if (i < len) {
            var pathname = path.join(dir, files[i]);
            fs.stat(pathname, (err, stats) => {
              if (err) {
                reject()
              }
              if (stats.isDirectory()) {
                travel(pathname, callBack, () => {
                  next(i + 1)
                })
              } else {
                fileList.push(pathname);
                if (typeof callBack === 'function') {
                  callBack({
                    path: path.relative(rootDir, pathname),
                    fullPath:  pathname,
                    filename: files[i],
                    dir: dir,
                  }, () => {
                    remainTimes--
                    next(i + 1)
                  })
                }
              }
            })
          } else {
            finish && finish();
            if (remainTimes <= 0) {
              resolve(fileList);
            }
          }
        })(0)
      })
    }

    travel(rootDir, callBack);
  })
}

module.exports = iterateFiles;
```