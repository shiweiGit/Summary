#
##javascript 单线程，同步执行（一个等一个，一个完成在执行下一个）  
##变量、函数声明提前：  
##变量定义会声明提前，赋值留在原地，后面会覆盖前面  
##函数是一等公民,会提前到函数之前,定义赋值同步提升  
##函数名指针，指向一个地址（存储空间）:堆内存 （先进先出）  
##函数执行会将上下文压入执行环境栈:栈内存(先进后出，后进先出)  
##变量和函数会在不用的时候，或者浏览器关闭的时候自动销毁  
##作用域：变量或函数起作用的区域（生命周期），分为全局及私有作用域，私有作用域内部变量（局部变量），外部无法访问  
##作用域链：如果当前有，使用自己的，如果当前没有，继续往上查找，形成的链式结构  
##匿名函数自调：形成私有作用域不受外界干扰改变内部变量，定义执行  
##this:指针对象，指向当前调用的对象，默认指向全局window  
##数据类型：基本、引用，基本是操作值，引用是操作地址  
##闭包：内部地址（私有作用域）被外部的变量引用，从而无法释放内存，作用：延长生命周期，保护局部变量，外部可以获取函数内部变量，缺点：内存泄漏  

## javascript中一切皆对象  
## Object是基类  
## 对象都是通过函数创建的，函数是对象，对象不是函数  
## 函数的父类是对象,对象的父类是函数  
## javascript中一切函数实际都是函数对象，函数是一种特殊的对象，函数对象(Objcect、Function)有prototype和__proto__和prototype  
## Array,Number,String等这些内置对象是Object的子类，也是Function的子类  
		

		// 构造函数对象(类)存在prototype(原型对象)和__proto__(原型指针，指向所属的函数对象(类)的原型->Function prototype)
		// 构造函数对象(类)的prototype(原型对象）中存在constructor和__proto__(原型指针）
		// 构造函数对象(类)的prototype(原型对象）的constructor 指向当前构造函数
		// 构造函数对象(类)的prototype(原型对象）的__proto__(原型指针，指向所属的函数对象(类)的原型->Object prototype）
	 
		// Object 函数对象存在prototype(原型对象)和__proto__(原型指针，指向所属的函数对象(类)的原型->Function prototype
		// Object 函数对象存在prototype(原型对象)中存在constructor
		// Object prototype(原型对象)中的constructor 指向所属的函数对象(类)->Object
		// Object prototype(原型对象)中无__proto__,因为Object已经处于原型链最顶端，无法继续往上查找

		// Function 函数对象(类)存在prototype(原型对象)和__proto__(原型指针，指向所属的函数对象(类)的原型->Function prototype)
		// Function 函数对象(类)的prototype(原型对象)中存在constructor和__proto__(原型指针）
		// Function 函数对象(类)的prototype(原型对象)中的constructor 指向所属类->Function
		// Function 函数对象(类)的prototype(原型对象)中的__proto__(原型指针，指向所属的函数对象(类)的原型 ->Object prototype)

		
		function fn(a){
			this.a=a;
		}
		fn.prototype.getA=function(){//原型上存放共有方法
			console.log(this.a);
		}

		//实例对象的原型指针，指向所属类的原型-> fn.prototype，fn函数对象的原型对象中的原型指针__proto__，指向所属的函数对象（类）的原型->Object prototype
		// Object函数对象（类)的__proto__(原型指针，指向所属的函数对象（类）的原型 ->Function prototype)
		 var obj1 = new fn();//生成新对象
		 var obj2 = new fn();
		 var obj3 = new Function();
		 var obj4 = new Object();
		 // console.log(obj1.__proto__ == fn.prototype) //true 
		 // console.log(obj1.getA == obj2.getA);//true 
		 // console.log(fn instanceof Function)//true 
		 // console.log(fn instanceof Object)//true 构造函数，即函数对象(类)，即是函数，也是对象

		 // console.log(typeof(obj1)) //object
		 // console.log(obj1 instanceof Object)//true  
		 // console.log(obj1 instanceof Function)//false 对象不是函数

		 // console.log(obj2 instanceof Object)//true  
		 // console.log(obj2 instanceof Function)//false 对象不是函数

		 // console.log(typeof(obj3)) //function
		 // console.log(obj3 instanceof Object)//true  
		 // console.log(obj3 instanceof Function)//true  基于函数对象（类）实例化出的实例既是对象，也是函数 

 		//  console.log(typeof(obj4)) //object
		 // console.log(obj4 instanceof Object)//true  
		 // console.log(obj4 instanceof Function)//false  基于对象实例化出的实例是对象，不是函数 


		 // console.log(Function instanceof Object) 函数的父类是对象，对象是基类
		 // console.log(Object instanceof Function) 对象的父类是函数，对象通过函数创建
		
		// 类的实例的原型的名字就是当前函数对象(内置类)  
	    // Number string object function
	    // undefined null 没有
		var a=1;
		var b='1';
		var c=true;
		var d=undefined;
		var e=null;
		var obj={a:'a'};
		function fn(){
			console.log(0111)
		}
		
		console.log([1,2,3,4].constructor.name);//Array 
		console.log(a.constructor.name);//Number
		console.log(b.constructor.name);//String
		console.log(c.constructor.name);//Boolean
		console.log(d.constructor.name);//
		console.log(e.constructor.name);//
		console.log(obj.constructor.name);//Object
		console.log(fn.constructor.name);//Function
