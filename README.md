# javascript
## javascript 单线程，同步执行（一个等一个，一个完成在执行下一个）  
## 变量、函数声明提前：  
## 变量定义会声明提前，赋值留在原地，后面会覆盖前面  
## 函数是一等公民,会提前到函数之前,定义赋值同步提升  
## 函数名指针，指向一个地址（存储空间）:堆内存 （先进先出）  
## 函数执行会将上下文压入执行环境栈:栈内存(先进后出，后进先出)  
## 变量和函数会在不用的时候，或者浏览器关闭的时候自动销毁  
## 作用域：变量或函数起作用的区域（生命周期），分为全局及私有作用域，私有作用域内部变量（局部变量），外部无法访问  
## 作用域链：如果当前有，使用自己的，如果当前没有，继续往上查找，形成的链式结构  
## 匿名函数自调：形成私有作用域不受外界干扰改变内部变量，定义执行  
## this:指针对象，指向当前调用的对象，默认指向全局window  
## 数据类型：基本、引用，基本是操作值，引用是操作地址  
## 闭包：内部地址（私有作用域）被外部的变量引用，从而无法释放内存，作用：延长生命周期，保护局部变量，外部可以获取函数内部变量，缺点：内存泄漏  

## javascript中一切皆对象  
## Object是基类  
## 对象都是通过函数创建的，函数是对象，对象不是函数  
## 函数的父类是对象,对象的父类是函数  
## javascript中一切函数实际都是函数对象，函数是一种特殊的对象，函数对象(Objcect、Function)有prototype和__proto__和prototype  
## Array,Number,String等这些内置对象是Object的子类，也是Function的子类  
## proto__低版本IE浏览器，存在兼容问题	
## 对象有__proto(原型指针)，函数有prototype原型，函数对象既有prototype，也有__proto__
## 函数有arguments(类数组对象)，name（函数名，不常见：(undefined:匿名函数自调),("":无名函数),（anonymousnew:Function方式创建)，(bind 函数名：通过bind方式创建的函数))，length（缺省为形参长度，当形参有缺省值，length为当前有缺省值的形参下),call，apply，bind,caller属性,call，apply，bind方法是Function原型上的方法，是在Function原型上的一个方法，故所有实例化的对象都可以使用  
## arguments  
	arguments有callee属性
    作用：指向正被执行的 Function 对象，也就是函数本身，表示对函数对象本身的引用
    
    arguments有length属性
    arguments.length是实参长度
    arguments.callee.length是形参长度

	var fn= function(a){
		console.log(arguments.length);//2
		console.log(arguments.callee.length);//1
		return "fn"
	}
	console.log(fn(1,2));
## call是一个对象，是在Function原型上的一个方法，故所有函数都有call方法  
	作用：让函数执行，改变函数的this（指针），第一个参数为改变this的参数（不可以省略），第二个参数为接收参数

	var fn= function(a){
		console.log(this.name) // window.name缺省为空
	}
	fn();

	var obj={
		name:'shi'
	}

	fn.call(obj,1); //shi  fn函数执行，改变fn的this为obj
	----------------------------------------------------------------------
	var fn= function(){
		console.log(this); // Number对象实例，Number对象没有name属性
		console.log(typeof(this)); //object
		console.log(this instanceof Object); // true
		console.log(this instanceof Function);// false
		console.log(this instanceof Number);// true
		
	}
	// fn();

	var obj={
		name:'shi'
	}
	fn.call(obj,1)
	fn.call(1);//转换为包装类型new Number(1)去改变函数this，第一个参数不可以省略

## apply和call作用相当，区别为第二个参数是接收数组    
	fn.apply(obj,[1,2,3]); 
	
	//借助apply求数组最大值
	var ary=[1,2,3,4,5];
	var max = Math.max.apply(null,ary)
	console.log(max);
	  
## bind和call类似，改变this  
区别:call会执行，而bind不会
	call、bind 综合实例：

		function change(n){
			this.innerHTML=parseInt(this.innerHTML)+n;
		}

		box.onclick=function(){
			change.call(this,2)
		}

		box.onclick=function(){
			this.change = change;
			this.change(1);
		}

		// 借助第三个参数
		// setInterval(function(ele){
			// 	ele.change=change;
			// 	ele.change(1)
		// },1000,box)
		
		// 使用call改变change指针
		// setInterval(function(){
			// 	change.call(box,2)
		// },1000)

		//bind于call类似，改变this
		//区别：bind 不会执行
		setInterval(change.bind(box,1),1000)

## caller指向调用当前函数的引用，说明这个函数调用了当前函数   
## 检测数据类型  
	typeof 仅检查基本数据类型，null为object ,引用数据类型不可详细检测，为Object或Function
	instanceof 检测一个实例是否属于某个类，对于基本数据类型，使用字面量方式创建的结果为false,构造函数方式创建可以检测
	constructor	获取数据类型
		// var ary=[1,2,3,4,5];
		// console.dir(ary);
		// console.log(ary.__proto__);//数组通过new Array 方式创建，Array函数对象（类）的原型（__proto__）上有constructor 指向Array,所以construcotr.name 就是当前所属函数对象（类）的名字
		// console.log(ary.constructor.name);//Array
		
	Object.prototype.toString.call("1").slice(8,-1); // [object String].slice(8,-1)  -> String
## for in 遍历对象  
	遍历会将自有的和原型上的方法都遍历出来，需要使用hasOwnProperty过滤，只要私有的
	Object.getOwnPropertyDescriptor(FF.prototype,"counstructor")
	configurable: true //是否可配置
	enumerable: false//是否可枚举，true属性的可以使用for in
	value: ƒ ()
	writable: true //是否可写

## eval 函数可计算某个字符串,并执行其中的的 JavaScript 代码  
	改变数据类型
	// var aryStr='[1,2,3,4,5]';
	// var ary=[1,2,3,4,5];
	// var obj={a:1,b:2};
	// var objStr='{a:1,b:2}'

	// //字符串数组转为数组
	// console.log(eval(aryStr));

	// //字符串对象转为对象
	// console.log(eval("("+objStr+")"));  
	
##window 对象有JSON对象，JSON有两个方法    

	//将JSON对象转为JSON字符串
	JSON.stringify(obj)
	JSON.stringify(ary) //数组也是对象，也可以转
	
	//将JSON字符串转为JSON对象
	JSON.parse(str)
	
## 重写数组方法  
	//var ary=[1,2,3,4,5];

	//末尾出去一个,并返回当前弹出元素
	// Array.prototype.pop=function(){
	// 	var num = this[this.length-1];
	// 	this.length--;
	// 	return num;
	// }

	// console.log(ary);
	// ary.pop();
	// console.log(ary);

	//末尾进来一个或者多个
	// Array.prototype.push=function(){
	// 	for(var i=0;i<arguments.length;i++){
	// 		this[this.length] = arguments[i];
	// 	}
	// }

	// ary.push(4,5);
	// console.log(ary);

	
	// Array.prototype.shift=function(){
	// 	var num = this[0];
	// 	for(var i=1;i<this.length;i++){
	// 		this[i-1]=this[i];
	// 	}
	// 	this.length--;
	// 	return num;
	// }

	// //前面出去一个,并返回当前元素
	// ary.shift();
	// console.log(ary);

	
	// Array.prototype.unshift=function(){
	// 	var str='';
	//两数组拼接
	// 	for(var i=0;i<arguments.length;i++){
	// 		str+=arguments[i]+',';
	// 	}

	// 	for(var i=0;i<this.length;i++){
	// 		str+=this[i]+',';
	// 	}
	// 	字符串转换为数组
	// 	var ary= eval("["+str+"]");
	// 	this.length = ary.length;
	
	//将新数组赋值回原数组，达到改变原数组的目的
	// 	for(var i=0;i<ary.length;i++){
	// 		this[i]=ary[i];
	// 	}
	// 	return ary.length;
	// }

	// // //前面进来一个或者多个,并返回元素的长度
	//  console.log(ary.unshift(0,1));
	//  console.log(ary);
 
## 重写call方法  
	function fn(){
		console.log(this);
	}
	var obj={"name":"shi"};
	Function.prototype.call=function(){
		//参数为空执行，执行当前函数
		if(arguments == undefined){
			this();
		}else{
			//如果传进来的第一个参数不是对象，将第一个参数转换为对象
			var obj = Object(arguments[0]);
			//在对象所属类的原型上添加fn方法
			obj.__proto__.fn = this;
			//执行
			obj.fn();
			//使用完之后删除
			delete obj.fn;
		}
	}
	fn.call(obj,1,2,3);

## 定时器是异步的，定时器使用完，不在使用的时候清除定时器  		
### 下面通过一个实例说明一下对象与函数的关系
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
