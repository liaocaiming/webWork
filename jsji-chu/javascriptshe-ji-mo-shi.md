#### 一. 考一些继承的方式
###### 有一个Animal类，拥有name，age属性和speak方法

```
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