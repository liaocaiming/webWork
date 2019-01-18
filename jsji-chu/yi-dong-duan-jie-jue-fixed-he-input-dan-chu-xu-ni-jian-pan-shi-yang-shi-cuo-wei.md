### 因为是输入的时候出现的问题，可以在输入时改变fixed属性为static,于是用到focus事件。

```js
$('#telephone').bind("focus",function(){
   $(".div_ft").css({"position":"static","bottom":0});
}).bind("blur",function(){
  $(".div_ft").css("position","fixed");
});
```

### 然后发现在屏幕翻转的时候还是不行，于是还要加入屏幕翻转监听事件
```js
// 判断屏幕是否旋转
$(window).bind("onorientationchange",function(){
    switch(window.orientation) {
        case 0:
            $('.weui-foter').css({'position':'fixed','bottom':'0'});
            break;
        case -90:
            $('.weui-foter').css({'position':'static'});
            break;
        case 90:
            $('.weui-foter').css({'position':'static'});
            break;
        case 180:
            $('.weui-foter').css({'position':'fixed','bottom':'0'});
            break;
    }
})
```