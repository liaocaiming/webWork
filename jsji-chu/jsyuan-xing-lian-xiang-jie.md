### 一. js原型链详解:

![](/assets/opo.png)

### 二. Object.create();
var obj =  {a: 1, b: 22};
var obj1 = Object.create(obj);
obj1.__proto__ === obj; // true
obj1继承了obj;
 