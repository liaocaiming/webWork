#### 加载更多函数
```js
function loadMore (elem, callBack) {
  let [scrollTop, clientHeight, scrollHeight] = [elem.scrollTop, elem.clientHeight, elem.scrollHeight]
  let diffHeight = scrollHeight - clientHeight - scrollTop
  if (diffHeight < 10 && scrollTop > 0) {
    callBack && callBack()
  }
}
```
#### 使用
```js
loadMore (paraId, () => {
  int++ // 加载的page
  isRequest = false // ajax是否请求回来
  if (int > totalPage) {
    $toast.show({
      type: 'text',
      text: '没有更多了',
      time: 500
    })
    return
  }
  getData(int)
})
```