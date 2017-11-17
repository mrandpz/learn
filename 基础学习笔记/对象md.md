Proxy  翻译代理
	
	Proxy接受两个参数。第一个参数是所要代理的目标对象（例是一个空对象），即如果没有Proxy的介入，操作原来要访问的就是这个对象；第二个参数是一个配置对象，对于每一个被代理的操作，需要提供一个对应的处理函数，该函数将拦截对应的操作。
	var proxy = new Proxy({}, { // 
	  get: function(target, property) {
	    return 35;
	  }
	});
	
	proxy.time // 35
	proxy.name // 35
	proxy.title // 35



Proxy 实例也可以作为其他对象的原型对象。

	var proxy = new Proxy({}, {
	  get: function(target, property) {
	    return 35;
	  }
	});
	
	let obj = Object.create(proxy);
	obj.time // 35

	obj 的 上一个原型链 是 proxy所以根据原型链，可以找到time

如果一个属性不可配置（configurable）和不可写（writable），则该属性不能被代理，通过 Proxy 对象访问该属性会报错。

	const target = Object.defineProperties({}, {
	  foo: {
	    value: 123,
	    writable: false,
	    configurable: false
	  },
	});
	
	const handler = {
	  get(target, propKey) {
	    return 'abc';
	  }
	};
	
	const proxy = new Proxy(target, handler);
	
	proxy.foo
	// TypeError: Invariant check failed