
    
### 1.addClass:为指定的dom元素添加样式 removeClass:删除指定dom元素的样式 toggleClass:如果存在(不存在)，就删除(添加)一个样式 hasClass:判断样式是否存在

```js
style type="text/css">  
    div.testClass{  
        background-color:gray;  
    }  
</style>  
  
<script type="text/javascript">  
function hasClass(obj, cls) {  
    return obj.className.match(new RegExp('(\\s|^)' + cls + '(\\s|$)'));  
}  
  
function addClass(obj, cls) {  
    if (!this.hasClass(obj, cls)) obj.className += " " + cls;  
}  
  
function removeClass(obj, cls) {  
    if (hasClass(obj, cls)) {  
        var reg = new RegExp('(\\s|^)' + cls + '(\\s|$)');  
        obj.className = obj.className.replace(reg, ' ');  
    }  
}  
  
function toggleClass(obj,cls){  
    if(hasClass(obj,cls)){  
        removeClass(obj, cls);  
    }else{  
        addClass(obj, cls);  
    }  
}  
  
function toggleClassTest(){  
    var obj = document. getElementById('test');  
    toggleClass(obj,"testClass");  
}  
</script>  
  
<body>  
    <div id = "test" style = "width:250px;height:100px;">  
        sssssssssssss  
    </div>  
    <input type = "button" value = "toggleClassTest" onclick = "toggleClassTest();" />  
</body>  
```

## 2. for ...in  这个遍历器会遍历到对象自己原型上的数据;

```
var obj ={
    a: 1,
    c: 2
}
obj.__proto__.d = 5;
for(var k in obj) {
    console.log(k)
}
// 结果: a   c    d

```

#### 正确写法;
```
for(var k in obj) {
    if (obj.hasOwnProperty(k)) {
        console.log(k)
    }
}
```
例如重写Object.values()方法:

```
if (!Object.values) {
    window.Object.values = function (obj) {
        let arr = [];
        for(var k in obj) {
            if(obj.hasOwnProperty(k)) {
                arr.push(obj[k])
            }
        }
    }
}
```
   