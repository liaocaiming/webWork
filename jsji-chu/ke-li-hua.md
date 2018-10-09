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


