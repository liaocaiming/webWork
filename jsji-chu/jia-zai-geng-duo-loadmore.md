```
function loadMore (elem, callBack) {
      let [scrollTop, clientHeight, scrollHeight] = [elem.scrollTop, elem.clientHeight, elem.scrollHeight]
      let diffHeight = scrollHeight - clientHeight - scrollTop
      if (diffHeight < 10 && scrollTop > 0) {
        callBack && callBack()
      }
}
```