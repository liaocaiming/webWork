### 实现add(1)(2)(3)

#### 什么是柯里化（Currying）
又称部分求值（Partial Evaluation），简单来说就是只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
```
var sumCurrying = function(a) {
    return function(b) {
        return a + b;
    }
}

var first = sum(1); //function
var second = sum(2); //function

sumCurrying(1)(2); //3
first(3); //4
second(4); //6
```
定义一个sumCurrying函数，接受一个参数并返回一个新的函数。调用sumCurrying函数之后，返回的函数就通过闭包的方式记住了sumCurrying的第一个参数。

但是这里只能接受两个参数，如果能接受很多个参数怎么办呢？

### 柯里化的运用
```
function add () {
    var args = [].slice.call(arguments);

    var fn = function () {
        var arg_fn = [].slice.call(arguments);
        return add.apply(null, args.concat(arg_fn));
    }

    fn.valueOf = function() {
        return args.reduce((a, b) => a + b);
    }
    return fn;
}
```

我第一次看这个代码的时候我也是纳闷，what the fuck？？valueOf是什么鬼？它运行了吗？原来valueOf和toString在某些时候是会自己调用的，还牵扯到了类型转换，啊啊啊啊~。

来看看这个类型转换的题

2 == [[[2]]] //是真还是假？

解析：

引用类型转换为基本类型(所有的引用类型转换为布尔值都是true)

引用类型转换为字符串

1.优先调用toString方法（如果有），看其返回结果是否是原始类型，如果是，转化为字符串，返回。 
2.否则，调用valueOf方法（如果有），看其返回结果是否是原始类型，如果是，转化为字符串，返回。 
3.其他报错。

引用类型转化为数字

1.优先调用valueOf方法（如果有），看其返回结果是否是原始类型，如果是，转化为数字，返回。 
2.否则，调用toString方法（如果有），看其返回结果是否是原始类型，如果是，转化为数字，返回。
3.其他报错。

```
以下来自犀牛书：

1，null和undefined，相等。 
2，数字和字符串，转化为数字再比较。 
3，如果有true或false，转换为1或0，再比较。 
4，如果有引用类型，优先调用valueOf。 
5，其余都不相等。
```

回到类型转换的这个题，因为是与数字进行比较，所以先调用valueOf()，返回的是本身不是原始类型，然后调用toString()得到"2"，然后变成了字符串和数字（"2" == 2）进行比较。

所以结果为 true

好，回到valueOf和toString被自动调用的问题上。

```
var obj = {name: 'Coco'};
var str = '123' + obj; 
console.log(str); //123[object Object]
```
为什么会这样呢？上面的例子实际上是
