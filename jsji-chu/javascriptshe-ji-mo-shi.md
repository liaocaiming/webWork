#### 一. 考一些继承的方式
###### 有一个Animal类，拥有name，age属性和speak方法

```js
       function Animal (name,age) {
	    	this.name = name;
	    	this.age = age;
	    	//当自己本身有这个变量时，就不会根据原型链来查找这个变量了，意思就是原型上的这个方法或者属性被屏蔽了，再也找不到了；
	    	this.speak = function () {
	    		console.log('nimei')
	    	}
	    }
	    Animal.prototype = {
	    	speak: function () {
	    		console.log('nihao');
	    	}
	    	changColor: function () {
	    		console.log('red')
	    	}
	    }

```
######  现在想定义一个Dog类，想继承的Animal的属性和方法，并且定义一个run方法，每隔一秒能执行一次speak方法；（使用构造函数继承和原型继承

```js
//构造函数式继承，继承构造函数Animal内部的属性和方法；
	    function Dog (name,age) {
	    	Animal.apply(this,[name, age]);
	    	this.color = 'red';
	    }
	    //原型继承，继承Animal原型上的一些方法和属性；
	    Dog.prototype = new Animal();
        Dog.prototype.contructor = Dog;
	    Dog.prototype.run = function () {
	    	var that = this;
	    	var num = 0;
	    	var timer = setInterval(function () {
                 that.speak();
                 num++;
                 if (num > 4) {
                 	clearInterval(timer);
                 };
	    	}, 30)
	    }
	    var xiaobai = new Dog('xiaobai', 11);
	    xiaobai.say();
	    console.log(xiaobai)
```

 ##### 注意：用构造函数继承时会出现一些问题：如父类的构造函数内部的属性和方法会执行两次
 Animal.apply(this,[name, age])和Dog.prototype = new Animal();解决方法用寄生组合继承
 
 ```
       function Aanimal (name, age) {
        	this.name = name;
        	this.age =age;
        }
        Aanimal.prototype.speak = function () {
        	console.log('hello');
        }
        //构造函数式继承；
        function Dog (name, age) {
        	Aanimal.call(this, name, age)
        };
        //原型式继承；
        function object (o) {
        	function F () {};
        	F.prototype = o;
        	return new F();
        }
        //
        function inheritPrototype (subType, superType) {
             // var prototype = object(superType.prototype);
             var prototype = Object.create(superType.prototype);//Object.create是基于原型式继承实现的；
             prototype.constructor = subType;
             subType.prototype = prototype;
        }
       inheritPrototype (Dog, Aanimal);
       Dog.prototype.run = function () {
	    	var that = this;
	    	var num = 0;
	    	var timer = setInterval(function () {
                 that.speak();
                 num++;
                 if (num > 4) {
                 	clearInterval(timer);
                 };
	    	}, 30)
	    }
     var xiaobai = new Dog('xiaobai', 222);
     xiaobai.run();
     console.log(xiaobai)
     console.log(xiaobai.__proto__.__proto__ === Aanimal.prototype)
 ```
 
