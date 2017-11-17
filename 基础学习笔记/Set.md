数组去重 向 Set 加入值的时候，不会发生类型转换，所以5和"5"是两个不同的值

	// 去除数组的重复成员
	[...new Set(array)]

两个对象总是不相等的。

	let set = new Set();

	set.add({});
	set.size // 1
	
	set.add({});
	set.size // 2


Set 结构的实例有以下属性。

- Set.prototype.constructor：构造函数，默认就是Set函数。  
- Set.prototype.size：返回Set实例的成员总数。


方法分为两大类：
**操作方法（用于操作数据）**和**遍历方法（用于遍历成员）**。  
下面先介绍四个操作方法。

- add(value)：添加某个值，返回Set结构本身。
- delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
- has(value)：返回一个布尔值，表示该值是否为Set的成员。
- clear()：清除所有成员，没有返回值。

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- keys()：返回键名的遍历器
- values()：返回键值的遍历器
- entries()：返回键值对的遍历器
- forEach()：使用回调函数遍历每个成员

Set 可以很容易地实现并集（Union）、交集（Intersect）和差集（Difference）。

	let a = new Set([1, 2, 3]);
	let b = new Set([4, 3, 2]);
	
	// 并集
	let union = new Set([...a, ...b]);
	// Set {1, 2, 3, 4}
	
	// 交集
	let intersect = new Set([...a].filter(x => b.has(x)));
	// set {2, 3}
	
	// 差集
	let difference = new Set([...a].filter(x => !b.has(x)));
	// Set {1}


Map  
Object 结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

	const map = new Map([
	  ['name', '张三'],
	  ['title', 'Author']
	]);
	
	Map {"name" => "张三", "title" => "Author"}

	const map = new Map([
	  ['name', '张三'],
	  ['title', 'Author']
	]);
	
	map.size // 2
	map.has('name') // true
	map.get('name') // "张三"
	map.has('title') // true
	map.get('title') // "Author"

任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构.都可以当作Map构造函数的参数。这就是说，Set和Map都可以用来生成新的 Map。
	
	const map = new Map();

	const k1 = ['a'];
	const k2 = ['a'];
	
	map
	.set(k1, 111)
	.set(k2, 222);
	
	map.get(k1) // 111
	map.get(k2) // 222

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- 
- keys()：返回键名的遍历器。
- values()：返回键值的遍历器。
- entries()：返回所有成员的遍历器。
- forEach()：遍历 Map 的所有成员。


	const map = new Map([
	  ['F', 'no'],
	  ['T',  'yes'],
	]);
	
	for (let key of map.keys()) {
	  console.log(key);
	}
	// "F"
	// "T"
	
	for (let value of map.values()) {
	  console.log(value);
	}
	// "no"
	// "yes"
	
	for (let item of map.entries()) {
	  console.log(item[0], item[1]);
	}
	// "F" "no"
	// "T" "yes"
	
	// 或者
	for (let [key, value] of map.entries()) {
	  console.log(key, value);
	}
	// "F" "no"
	// "T" "yes"
	
	// 等同于使用map.entries()
	for (let [key, value] of map) {
	  console.log(key, value);
	}
	// "F" "no"
	// "T" "yes"