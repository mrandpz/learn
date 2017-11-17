1.暂时性死区  
	
		var tmp = 123;
		if (true) {
			tmp = 'abc';
			let tmp; //tmp is not defined  暂时性死区结束
			console.log(tmp);  // 如果没有上面的tmp = 'abc';这一行将输出undefined，否则连下面都无法输出

			tmp = '123';
			console.log(tmp)
		}
	
“暂时性死区”也意味着typeof不再是一个百分之百安全的操作。
     
	typeof x; // ReferenceError
	let x;

作为比较，如果一个变量根本没有被声明，使用typeof反而不会报错。
	

	typeof undeclared_variable // "undefined"

隐蔽的死区-- 所以函数内的赋值也是一个类似let const 的赋值
	
		function bar(x=y,y=2) {
			return [x,y]
		}
		bar() // y is not defined

		function bar(x=2,y=x) {
			return [x,y]
		}
		bar() // [2, 2]

2.不允许重复声明  不允许在**相同作用域内**，重复声明同一个变量

		function func() {
			let a = 10;
			var a = 1;// Identifier 'a' has already been declared
		}
		
		function func() {
		  let a = 10;
		  let a = 1;// Identifier 'a' has already been declared
		}
		// 不可在函数内部重复声明
		function func(arg) {
			let arg
		}

		function func(arg) {
		  {
		    let arg; // 不报错 此时的作用域不同
		  }
		}

3.为什么需要块级作用域
	
- 变量提升 
- 全局变量--闭包	

		var tmp = new Date();
		function f() {
		  console.log(tmp); // undefined
		  if (false) {
		  	console.log(11) // 不执行这段函数
		    var tmp = 'hello world'; // 相当于把tmp 提升到console.log之前，再这个作用域内重新声明了tmp，也就是变量提升
		  }
		}

		f();

		var s = 'hello';
		for (var i = 0; i < s.length; i++) {
		  console.log(s[i]);
		}
		console.log(i); // 5
		
4.es6的块级作用域  let决定
	
	function f1() {
	  let n = 5;
	  if (true) {
	    let n = 10;
	  }
	  console.log(n); // 5
	}

	function f1() {
	  var n = 5;
	  if (true) {
	    var n = 10;
	  }
	  console.log(n); // 10
	}

	可替代IIFE 写法
	// IIFE 写法
	(function () {
	  var tmp = ...;
	  ...
	}());
	
	// 块级作用域写法
	{
	  let tmp = ...;
	  ...
	}

5.函数声明在es6中  
	ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。
	

	function f() { console.log('I am outside!'); }

	(function () {
	  if (false) {
	    // 重复声明一次函数f
	    function f() { console.log('I am inside!'); }
	  }
	
	  f();
	}());

	// ES5 环境, 上述代码在es5环境中和在es6环境中是不一样的
	// ES5 中由于函数声明提升得到的结果是 i am inside
	function f() { console.log('I am outside!'); }
	
	(function () {
	  function f() { console.log('I am inside!'); }
	  if (false) {
	  }
	  f();
	}());

	// ES6 chrom 中报错 ie中 打印es5环境
	function f() { console.log('I am outside!'); }

	(function () {
	  if (false) {
	    // 重复声明一次函数f
	    function f() { console.log('I am inside!'); }
	  }

	  f(); //f is not a function
	}());

**考虑到环境因素差异，应该避免在块级作用域内声明函数，如果确实需要，也应该写成函数表达式，而不是函数声明语句**

6.do 语句 -- 提案---返回最后一行--暂不支持
	
		let x = do {
		  let t = f();
		  t * t + 1;
		};

7.const 命令
	const声明一个只读的敞亮，一旦声明，常量的值就不能改变，而且一旦声明就必须初始化，不能留到以后赋值
	
	const PI = 3.1415;
	PI // 3.1415
	
	PI = 3;
	// TypeError: Assignment to constant variable.

	const foo;
	// SyntaxError: Missing initializer in const declaration

8.const 变量的本质 -- **变量指向的那个内存地址不得改动**
	
	
	const foo = {};
	
	// 为 foo 添加一个属性，可以成功
	foo.prop = 123;
	foo.prop // 123 没有改变地址
	
	// 将 foo 指向另一个对象，就会报错
	foo = {}; // TypeError: "foo" is read-only

	const foo = Object.freeze({});

	// 常规模式时，下面一行不起作用；
	// 严格模式时，该行会报错
	foo.prop = 123;

 -   六种声明变量的方法 es中的 var function es中的let const import class  
 
 9.
	
	var a = 1;
	// 如果在 Node 的 REPL 环境，可以写成 global.a
	// 或者采用通用方法，写成 this.a
	window.a // 1
	let b = 1;
	window.b // undefined