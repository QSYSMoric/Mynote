## 1、数据类型
+ 原始类型：Undefined、Null、Boolean、Number、String、Symbol【ES6】、Bigint【ES10】
+ NaN是一个数值
+ 复杂类型：Object、Array、Function
## 2、数据类型转换
+ Number、String、Boolean
## 3、数据类型判断
+ **typeof**：
```js
console.log(typeof 1);　　        // number
console.log(typeof '1');　　      // string
console.log(typeof true );       // boolean
console.log(typeof undefined);   // undefined
console.log(typeof null);        // object
console.log(typeof {});          // object
console.log(typeof [] );         // object  

var func = function(){
    return 1
}
console.log(typeof func );       // object
console.log(typeof func() );     // number
console.log(typeof (function() {}));   // function
```
+ **constructor**
```js
console.log(false.constructor === Boolean); // true
```
+ **Object.prototype.toString.call**
```js
console.log(Object.prototype.toString.call(false)); // [object Boolean]
```
+ **instanceof**
```js
console.log([] instanceof Array);       // true
console.log({} instanceof Object);      // true  
var func = function(){
    return 1
}
console.log(func instanceof Function);  // true
console.log(1 instanceof Number);       // false
```
## 4、作用域和作用域链
+ 作用域就是代码的执行上下文，全局执行环境就是全局作用域，函数的执行环境就是私用作用域，他们都是栈内存
+ 当代码执行在一个环境中执行的时候，会创建一个作用域链
+ 当在内部函数中需要访问一个遍历的时候，首先会访问本作用域块，如果本函数作用域块没有该变量的时候，那么便会向外层的作用域寻找变量
## 5、深拷贝与浅拷贝方法
+ 浅拷贝：
	1. Object.assign
	2. 扩展运算符
	3. for...in
+ 深拷贝
	1. JSON对象来实现深拷贝【缺点：函数无法拷贝，会显示undefined】
	2. lodash函数库实现深拷贝（第三方库）
	3. 递归
## 6、闭包
+ 闭包指的是引用另外一个函数作用域中变量的函数，通常是在嵌套函数中实现。
+ 闭包满足：返回一个函数，这个函数中的引用另一个函数作用域中的变量，这些变量由于保持着引用因此不会被js垃圾回收释放
## 7、继承的几种方式
+ 对象继承的本质我认为就是对象的复制，因为js设计的时候就没有考虑到面向对象的方式，为了满足日后的需要那么便设计出了原型来解决对象继承
1. 构造继承
2. 原型链继承
3. 拷贝继承
4. 实例继承
5. 组合继承
6. 寄生组合继承
## 8、this、call、apply、bind
1. this的指向受函数运行环境的影响(默认指向window)
2. call方法可以修改this的指向(指向函数执行时所在的作用域), 然后再执行函数
3. apply和call作用类似，唯一区别是apply 接收数组作为函数执行时的参数
4. bind 用于将函数体内的this绑定到某个对象，然后返回一个新函数
## 9、事件捕获与事件冒泡
1. 捕获型事件：事件从document对象开始触发，然后到目标事件
2. 冒泡型事件：目标事件到document对象的顺序触发。
## 10、延迟加载js
+ 动态加载
+ 延时加载
+ async：与HTML同步解析，不是顺次执行
+ defer：等HTML全部解析完毕再执行js代码，顺次执行
## 11、null与undefined区别
+ 我所阅读的文献之中作者在设计时候先设计了一个null但是在后面js没有报错机制之前null总是会被转换为0因此，为了解决这一设计缺陷后面又设计出来一个undefined配合使用表示值未定义，而null表示指向一个对象但未指向
+ null表示一个无的对象，对应的是一个对象但是没有给指向，转换数值为0
+ undefined表示对应一个值，转为数值为NaN
## 12、== 与 === 区别
+ \=\=：比较的是值
	+ String == number || bollean || number ... 都会隐式转换
	+ 通过ValueOf转换，valueOf => toString => Object.toString
+ \=== ：不但比较值，而且还比较类型 
+ 总的来讲`==`如果双方是同一个数据类型会调用比较数据的valueOf()方法，比较值，例如两个对象比较的是引用地址number比较值，但是如果比较双方类型不同那么会进行隐式转化
+ 全等会先进行类型比较如果类型比较不通过那么不会进行下一步
## 13、微任务与宏任务
+ 在 JavaScript 中，事件循环（Event Loop）是处理异步操作的机制。微任务（microtask）和宏任务（macrotask）是用来管理和调度异步任务的两种不同的任务队列。
+ 宏任务是指由浏览器提供的一类任务，通常包括：
	- setTimeout 和 setInterval 的回调函数
	- DOM 事件回调
	- 用户交互事件回调（例如鼠标点击、滚动等）
	- XMLHttpRequest 和 fetch 的回调函数
+ 微任务是一种高优先级的任务，通常包括：
	- Promise 的回调函数
	- MutationObserver 的回调函数
## 14、对象
+ 对象是通过new操作符构建的，所以对象之间不相等
+ 对象是引用类型
+ 对象的key都是字符类型
## 15、判断对象是否为数组
1. Array.isArray()
2. instenceOf
3. 原型链
4. Object.prototype.toString.call()
## 16、array.slice arr.splice是否改变原数组
+  slice：作用：用于截取数组
	+ 返回一个新数组
+ splice：插入、删除、替换
	+ 会改变原数组
## 17、数组去重
1. Array.form(new Set()); || \[...new Set()\];
2. Array.findIndex
3. 自定义函数
## 18、原型链
+ 解决问题：对象共享属性和共享方法
+ 对象本身->原型链->返回undefined
## 19、JS的继承方式
+ es6的extend继承
+ 原型链继承
+ 寄生式继承
+ 组合式继承
+ 寄生式组合继承
## 20、String方法
### 20.1、length
+ 获取字符串长度
### 20.2、获取字符串指定位置的值
+ charAt()和charCodeAt()方法都可以通过索引来获取指定位置的值
### 20.3、检索字符串是否包含特定序列
+ indexOf( )：查找某个字符的下标
+ lastIndexOf( )：查找某个字符最后一次匹配的下标
+ includes( )：判断字符串是否包含指定子字符串
+ startsWidth( )：用于检测子字符串是否以指定的开始
+ endsWidth( )：用于检测是否以指定字符串结尾
### 20.4、分割字符串成数组
+ split( )：将一个字符串分割为字符串数组
+ 不会改变原始数组
### 20.5、截取字符串
+ string.slice(start,end);
+ string.substr();
+ string.substring();
### 20.6、字符串大小写切换
+ toLowerCase() 和 toUpperCase()
### 20.7、字符串模式匹配
+ replace()、match()和search()方法可以用来匹配或者替换字符。
## 21、isNaN和number.isNaN
+ isNaN( )：先尝试转换为数字，若无法转为数字，返回true，否则false
+ Number.isNaN( )：直接检查是否为一个NaN