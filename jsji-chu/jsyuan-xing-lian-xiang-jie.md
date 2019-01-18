### 一. js原型链详解:

![](/assets/opo.png)

### 二. Object.create();
var obj =  {a: 1, b: 22};
var obj1 = Object.create(obj);
obj1.__proto__ === obj; // true
obj1继承了obj;
 
 ####  三. 同一个构造函数new 出来的两个实例, 调用自身(非prototype上)的方法时,这两个方法不是同一个函数,它们储存的地址是不一样的,  它们只是两个业务一样的函数 !!!!
 
 ```js
 function People(){
   this.fun = function(){
     console.log(1111)
   }
 }
 
var xiaoming = new People();
var xiaohong = new People();
console.log(xiaoming.fun == xiaohong.fun)//false
 ```
 #### 四. 一般方法都定义在prototye上,此时同一个构造函数new 出来的两个实例调用自身(prototype上)的方法时时同一个函数
 
 ```
function People(){
	        
}
People.prototype.fun = function(){
  console.log(11111)
}

var xiaoming = new People();
var xiaohong = new People();
console.log(xiaoming.fun == xiaohong.fun)//true
 ```