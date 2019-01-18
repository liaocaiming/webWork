### 因为是输入的时候出现的问题，可以在输入时改变fixed属性为static,于是用到focus事件。

```js
$('#telephone').bind("focus",function(){
   $(".div_ft").css({"position":"static","bottom":0});
}).bind("blur",function(){
  $(".div_ft").css("position","fixed");
});
```