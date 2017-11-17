Symbol  
- 凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。  
- Symbol函数前不能使用new命令，否则会报错  
- Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
	
	let s = Symbol();

	typeof s
	// "symbol"

Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的。

	// 没有参数的情况
	let s1 = Symbol();
	let s2 = Symbol();
	
	s1 === s2 // false
	
	// 有参数的情况
	let s1 = Symbol('foo');
	let s2 = Symbol('foo');
	
	s1 === s2 // false

Symbol 值不能与其他类型的值进行运算，会报错

	let sym = Symbol('My symbol');

	"your symbol is " + sym
	// TypeError: can't convert symbol to string
	`your symbol is ${sym}`
	// TypeError: can't convert symbol to string

Symbol 值也可以转为布尔值，但是不能转为数值。
	
	let sym = Symbol();
	Boolean(sym) // true
	!sym  // false
	
	if (sym) {
	  // ...
	}
	
	Number(sym) // TypeError
	sym + 2 // TypeError

Symbol 值作为对象属性名时，不能用点运算符。

	因为点运算符后面总是字符串，所以不会读取mySymbol作为标识名所指代的那个值
	const mySymbol = Symbol();
	const a = {};
	
	a.mySymbol = 'Hello!';
	a[mySymbol] // undefined
	a['mySymbol'] // "Hello!"

	同理，在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。
	let s = Symbol();

	let obj = {
	  [s]: function (arg) { ... }
	};
	
	obj[s](123);

Symbol的获取

Symbol 作为属性名，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol 属性名。

**Reflect.ownKeys** 方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

	let obj = {
	  [Symbol('my_key')]: 1,
	  enum: 2,
	  nonEnum: 3
	};
	
	Reflect.ownKeys(obj)
	//  ["enum", "nonEnum", Symbol(my_key)]


	
Symbol.for()
	
	let s1 = Symbol.for('foo');
	let s2 = Symbol.for('foo');
	
	s1 === s2 // true
	
	Symbol.for("bar") === Symbol.for("bar")
	// true
	
	Symbol("bar") === Symbol("bar")
	// false

