#### 加载更多函数
```
function loadMore (elem, callBack) {
  let [scrollTop, clientHeight, scrollHeight] = [elem.scrollTop, elem.clientHeight, elem.scrollHeight]
  let diffHeight = scrollHeight - clientHeight - scrollTop
  if (diffHeight < 10 && scrollTop > 0) {
    callBack && callBack()
  }
}
```
#### 使用
```
loadMore (paraId, () => {
  int++
  this.isRequest = false
  if (this.int > this.page.totalPage) {
    this.$vux.toast.show({
      type: 'text',
      text: '没有更多了',
      time: 500
    })
    return
  }
  getData(int)
})
```