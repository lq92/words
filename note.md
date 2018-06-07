### 原生JS
    1. 判断类型是否是对象
      `obj.constructor === Object` 缺点：通过构造函数创建的对象返回false
      `typeof obj === 'object'` 缺点：null也会返回true
      `Object.prototype.toString.call(obj) === '[object Object]'`
    2. 箭头函数的this指向——箭头函数没有自己的this，是继承而来的，是定义时绑定(即this是指向父执行上下文中的this)，而不是运行时绑定
      ```
        var x = 11;
        function Test(){
            this.x = 22;
            function inner(){
                return this.x
            }
            inner()
        }
      ```
    3. 'use strict'——是一种在运行时自动执行更严格的js代码解析和错误处理的方法，如果代码错误被忽略或失败，将会产生错误或抛出异常
      * 防止意外全局变量，严格模式下未声明赋值的变量会报错
      * 重复声明的函数形参会报错
      * 严格模式下this指向undefined，非严格模式下指向window
      * 严格模式下禁用with语句
      * 严格模式下的eval不再为上层范围引入新变量
      * eval和arguments不能被重新赋值
      * 不能使用arguments.caller和arguments.callee
      * 不能使用0开头表示八进制
      * 不能删除不可删除的属性
      * 不能对只读属性赋值
    4. isNaN()和Number.isNaN()区别：isNaN会对非数字参数先转换成数字再判断
    5. 对象的深浅拷贝
      浅拷贝：Object.assign()将所有可枚举属性的值从一个或多个源对象复制到目标对象，返回目标对象
        ```
            let obj = {
                a: 1,
                b: [2, 3, 4],
                c: {
                    d: 5
                }
            }
            let o = Object.assign({}, obj)
        ```
      模拟underscore中extend(浅拷贝)实现
        ```
          function isObject(obj){ // 判断参数是否为对象，保留function，去除null
            let type = typeof obj;
            return type === 'function' || type === 'object' && !!obj;
          }
          function ownKeys(obj){ // 返回参数的所有键
            let keys = [];
            if(!isObject(obj)) return []; // 不是对象的话，返回[]
            if(Object.keys){
              return Object.keys(obj);
            }else{
              for(let i in obj){
                if(obj.hasOwnProperty(i)){ // 不获取原型链中的键
                  keys.push(i);
                }
              }
            }
            return keys;
          }
          function extend(target, ...sources){
            sources.forEach(source => {
              ownKeys(source).forEach(key => {
                target[key] = source[key];
              })
            })
            return target;
          } 
        ```
      深拷贝：浅拷贝对于数组和对象只拷贝引用的地址值
        ```
          function deepCopy(source, target = {}){
              for(i in source){
                  if(typeof source[i] === 'object'){
                      if(Array.isArray(source[i])){
                          target[i] = [];
                      }else{
                          target[i] = {};
                      };
                      deepCopy(source[i], target[i]);
                  }else{
                      target[i] = source[i];
                  };
              };
              return target;
          };
        ```
        JSON方法实现
        ```
        	function deepClone(target){
        		return JSON.parse(JSON.stringify(target)); // JSON.stringify将JSON对象解析为字符串，这时会在内存中开辟一个新的地址用来存储字符串，此时已经切断和原对象的关联，但是不符合JSON数据类型的不会拷贝，undefined和function以及Symbol会忽略
        	}
        ```
    6. 声明提升——变量和函数声明会提升到执行环境的顶部，在编译阶段发生，变量只提升声明，赋值操作会留在原地
      ```
        if(false){
            var a = 10;
        }
        console.log(a)  // undefined
      ```
      声明顺序：
        1、找到所有的函数声明，初始化函数体，如有同名函数则会覆盖
        2、查找变量声明，初始化为undefined，如存在同名变量，则直接略过
      ```
      	function Foo(){
      		getName = function(){
      			alert(1);
      		}
      		return this;
      	}
      	Foo.getName = function(){
      		alert(2);
      	}
      	Foo.prototype.getName = function(){
      		alert(3)
      	}
      	var getName = function(){
      		alert(4);
      	}
      	function getName(){
      		alert(5)
      	}
      	Foo.getName()
      	getName()
      	Foo().getName()
      	getName()
      	new Foo.getName()
      	new Foo().getName()
      	new new Foo().getName()
      ```
      运算符优先级：()函数调用或括号、.运算符、[]数组下标访问运算符优先级最高；new运算符、一元运算符、typeof元素符优先级次之
      注：当new运算符后跟对象创建时优先级提高(即：new Object()比new Object优先级高)
      (https://www.v2ex.com/t/351261)
    7. Cookie/sessionStorage/localStorage/indexedDB(https://www.cnblogs.com/cencenyue/p/7604651.html)
    	* cookie——在浏览器和服务器之间来回传递，除非过期，否则一直存在，4k，一般用于状态管理
    	* sessionStorage——周期只存在会话打开(重新加载不影响)即使重新打开同一个页面也不共享数据，5M
    		方法：setItem(key, val)/getItem(key)/removeItem(key)/clear()/key(index)
    		属性：length
    	* localStorage——除非清除，否则周期一直存在，同源页面也共享数据，5M
    		方法：setItem(key, val)/getItem(key)/removeItem(key)/clear()/key(index)
    		属性：length
    		```
    			for(let i = 0; i < localStorage.length; i++){
    				console.log(localStorage.getItem(localStorage.key(i)))
    			}
    		```
    	* [IndexedDB](http://www.tfan.org/using-indexeddb/)——让应用在用户的浏览器内持久化存储数据的方法，提供了丰富的查询能力，使应用在在线和离线时都可以工作
    8. 跨域——由浏览器的同源策略引起的，即同一域名、端口号、协议才能通信
      同源策略限制以下几种行为：
        1. Cookie/localStorage/indexedDB无法获取
        2. DOM和JS对象无法获得
        3. Ajax请求不能发送
      解决方式：
        1. jsonp——img的src属性、link的href属性和script的src属性不存在跨域
          ```
            function jsonHandler(data){
              console.log(data)
            }
            let script = document.createElement('script'),
                url = 'https://api.douban.com/v2/book/search?q=javascript&count=1&callback=jsonHandler';
            script.src = url;
            document.body.appendChild(script)
          ```
          只能实现get请求
        2. 跨资源共享(CORS)——普通跨域请求，服务端只设置Access-Control-Allow-Origin即可，前端无需设置，若要带Cookie请求，前后端都要设置
          ```
            let xhr = new XMLHttpRequest();
            xhr.withCredentials = true; // 前端设置是否带cookie
            xhr.open(method, url);
            xhr.send();
            xhr.addEventListener('readystatechange', () => {
              if(xhr.readyState === 4 && xhr.status === 200){
                console.log(xhr.responeseText)
              }
            })
          ```
        3. postMessage
        4. WebSocket
        5. nodejs中间件代理跨域
        6. nginx反向代理
    9. JS事件循环——JS是单线程非阻塞脚本语言
      单线程意味着js代码在执行的时候只有一个主线程来处理所有任务
      非阻塞指当代码需要进行一项异步任务时，主线程会挂起这个任务，等异步任务返回结果的时候再根据一定规则去执行响应的回调
      JS中的变量会放置在内存中的不同位置：堆(heap)和栈(stack)，堆中存放着变量，栈中存放着基础类型的和对象的指针
      事件循环——JS主线程在执行代码的时候是按照同步代码调用的顺序执行的，当遇到异步代码时，会将其挂起，并继续执行同步代码，当异步结果返回时会将其放到事件队列中，当主线程空闲时会查看事件队列中是否有任务，如果有则取出执行异步任务，然后执行其中的同步任务，循环往复即形成事件循环
      事件队列分宏任务(setTimeout setInterval)和微任务(new Promise)，先执行微任务中的事件，再执行宏任务中的事件
    10. 原型和原型链
    11. 重绘和回流(https://juejin.im/post/5a9923e9518825558251c96a)
      重绘——当页面中元素样式的改变并不影响它在文档流中的位置时(color/background-color/visibility)，浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘
      回流——当Render Tree中部分或全部元素的尺寸、结构或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流
      回流比重绘消耗性能开支更大
      回流必将引起重绘，重绘不一定会引起回流
    12. 浏览器从加载到渲染的过程
      加载——1、浏览器根据输入的URI通过DNS解析目标的IP地址；2、浏览器对目标服务器发起HTTP请求；3、服务器处理请求并响应；4、浏览器得到返回内容
      渲染——1、浏览器根据请求的HTML生成DOM树；2、根据CSS生成CSSOM；3、将DOM和CSSOM整合形成Render Tree；4、浏览器根据Render Tree开始渲染和展示；5、遇到script标签会执行并阻塞渲染
    13. 性能优化——减少页面体积，提升网络加载；优化页面渲染
      减少页面体积，提升网络加载
        代码压缩(可以通过webpack等构建工具进行js和css代码压缩)，雪碧图
        使用CDN让资源加载更快
      优化页面渲染
        CSS放前，JS放后
        懒加载(图片懒加载、下拉加载)
        减少DOM查询，对DOM查询做缓存
        减少DOM操作，多个操作尽量合并在一起执行(DocumentFragment)
        事件节流
    14. Promise——用来解决异步回调地狱的问题
      promise中的错误具有冒泡性质，使用catch捕获错误
      finally不管promise是什么状态，都会执行的语句
      ```
        let promise = new Promise((resolve, reject) => {
          if(/* 异步代码成功 */){
            resolve(value)
          }else{
            reject(error) // 异步代码失败
          }
        })
        promise.then(value => {
          // success
        }, error => {
          // fail
        })
      ```
      promise异步加载图片
      ```
        function loadImage(url){
          return new Promise((resolve, reject) => {
            let img = new Image();
            img.addEventListener('load', () => {
              resolve(img);
            })
            img.addEventListener('error', () => {
              reject(Error('The url is not found!'));
            })
            img.src = url;
          })
        }
        loadImage('xxx').then(img => {
          document.body.appendChild(img)
        })
      ```
      promise Ajax请求
      ```
        function getJSON(url){
          return new Promise((resolve, reject) => {
            let xhr = new XMLHttpRequest();
            function handler(){
              if(this.readyState === 4 && this.status === 200){
                resolve(this.response);
              }else{
                reject(Error(`error: ${this.status}`));
              }
            }
            xhr.open('GET', url);
            xhr.addEventListener('readystatechange', handler);
            xhr.responseType = 'json';
            xhr.setRequestHeader('Accept', 'application/json');
            xhr.send();
          })
        }
      ```
      Promise.all([promise1, promise2, ...])接收一个promise数组，如果参数有不是promise则用Promise.resolve()转为promise再操作，最终Promise.all有两个状态: 1、所有的promise为fulfiled则为fulfiled；2、有一个为rejected则为rejected
      ```
        let p1 = new Promise(resolve => resolve('Success'))
        let p2 = new Promise((resolve, reject) => reject(new Error('error'))).catch(err => err)
        Promise.all([p1, p2]).then(result => console.log(result)).catch(err => console.log(err))
        // Promise.all的catch捕获不到错误，因为p2捕获了错误
      ```
      Promise.race([promise1, promise2, ...])最终的状态由参数中首先返回的状态决定
    15. flatten实现
      ```
        let arr = [[1, 2, [3, 4]], [[[[5]]]], 6, 7];
        function flatten(source, target){
          target = target || [];
          source.forEach(item => {
            if(Array.isArray(item)){
              flatten(item, target);
            }else{
              target.push(item);
            }
          })
          return target;
        }
        let a = flatten(arr)
      ```
      reduce实现
      ```
        function flatten(source){
          return source.reduce((prev, next) => {
            if(Array.isArray(next)){
              return prev.concat(flatten(next));
            }
            return prev.concat(next);
          }, [])
        }
      ```
      Generator实现
      ```
        function* flat(arr){
          for(let val of arr){
            if(val.constructor === Array){
              yield* flat(val);
            }else{
              yield val;
            }
          }
        }
        let target = [];
        for(let val of flat(arr)){
          target.push(val);
        }
      ```
      牛B的实现
      ```
      	let flatten = arr => Array.isArray(arr) ? [].concat(...arr.map(flatten)) : arr;
      ```
    16. generator
      ```
      // for of循环
      Object.prototype[Symbol.iterator] = function* (){
        let keys = Object.keys(this);
        for(let i = 0; i < keys.length; i++){
          yield { value: this[keys[i]], done: !this[keys[i]]}
        }
      }
      let obj = {
        name: 'bill',
        age: 23
      }
      for(let i of obj){
        console.log(i)
      }
      ```
      如果一个generator中要执行另一个generator要使用yield* functionname语句
      ```
      function* foo(){
        yield 'x';
        yield 'y';
      }
      function* bar(){
        yield 'a';
        yield* foo();
        yield 'b';
      }
      let g = bar();
      for(let i of g){
        console.log(i); // a, x, y, b
      }
      ```
      generator实现数组展开(flatten方法)
      ```
      function* flatten(arr){
        for(let i of arr){
          if(Array.isArray(i)){
            yield* flatten(i);
          }else{
            yield i;
          }
        }
      }
      let g = flatten(arr);
      [...g]
      ```
    17. class与es原型
      class中声明的方法都是在原型上声明的，且是不可枚举的
      ```
      class Person(){
        sayName(){
          return 'Bill';
        }
      }
      Object.keys(Person.prototype) // return []
      Object.getOwnPropertyNames(Person.prototype) // return ['sayName']
      ```
      声明在class中的代码默认严格模式
      class中要声明constructor方法，若没有则默认添加
      class表达式
      `let Person = class {}`
      立即执行的class
      ```
      let person = new class {
        constructor(name){
          this.name = name;
        }
      }('Bill')
      ```
      不存在类提升
      静态方法——方法前加static关键字，这个静态方法是类的方法而不是实例的方法
      ```
      class Person {
        static foo(){
          return 'foo';
        }
      }
      Person.foo() // return 'foo'
      new Person().foo() // throw error
      ```
      静态方法中包含this指的是类的this
      父类的静态方法可以被子类继承
      new.target检测函数是否通过new调用，普通函数中返回undefined
      子类中调用父类的super方法，this指向当前子类的实例
      ```
      class Parent {
        constructor(){
          this.x = 10;
        }
        print(){
          return this.x;
        }
      }
      class Child extends Parent {
        constructor(){
          super();
          this.x = 20;
        }
        print(){
          return super.print();
        }
      }
      new Child().print() // return 20
      ```
    18. 事件委托——利用事件冒泡将事件绑定在父元素上
    19. this的指向
      普通函数调用指向window
      非严格模式下指向window，严格模式下指向undefined
      对象方法调用指向调用的对象
      构造函数调用，指向返回的新对象
      通过call/bind/apply动态绑定的this，指向传入的对象
      箭头函数的this指向继承父级的对象
    20. new方法做了什么
      新创建一个对象，并将构造函数的作用域指向这个对象
      执行构造函数的代码
      返回这个对象
    21. bind实现
      ```
        Function.prototype.bind = Function.prototype.bind || function(){
          let args = Array.from(arguments);
          let context = args.shift();
          let that = this;
          return function(){
            let innerArgs = Array.from(arguments);
            return that.apply(context, args.concat(innerArgs))
          }
        }
      ```
    22. 页面加载海量数据，不阻塞UI渲染
      ```
        let ul = document.createElement('ul');
        document.body.appendChild(ul);
        ul.addEventListener('click', e => {
          if(e.target.tagName === 'LI'){
            console.log(e.target.innerText)
          }
        })
        function data(size){
          let arr = [];
          for(let i = 0; i < size; i++){
            let li = document.createElement('li');
            li.innerText = i;
            arr.push(li);
          }
          return arr;
        }
        let arr = data(10000);
        function chunk(size){
          let frag = document.createDocumentFragment();
          for(let i = 0; i < size; i++){
            let item = arr.shift();
            if(item){
              frag.appendChild(item)
            }
          }
          ul.appendChild(frag);
          setTimeout(chunk, 0, 4)
        }
        chunk(4)
      ```
  	23. 数组去重
      indexOf去重
      ```
        let unique = target => {
          let arr = [];
          target.forEach(item => {
            if(arr.indexOf(item) === -1){
              arr.push(item);
            }
          })
          return arr;
        }
      ```
      hash去重
      ```
        let unique = target => {
          let hash = {};
          let arr = [];
          target.forEach(item => {
            let tem = typeof item + item;
            if(!hash[tem]){
              arr.push(item);
              hash[tem] = true;
            }
          })
          return arr;
        }
      ```
      Set去重
      ```
        [...new Set{}]
      ```
    24. 防止对象修改
      * Object.preventExtensions()——对象无法添加新属性
        使用Object.isExtensible()——获取对象是否可扩展
      * Object.seal()——密封对象，不可扩展，不可配置
        使用Object.isSealed()——获取对象是否密封
      * Object.freeze()——冻结对象，不可扩展，不可配置，不可写
        使用Object.isFrozen()——判断对象是否冻结
    25. valueOf()
      ```
        let o1 = { name: 1 }, o2 = { name: 2 };
        o1 > o2 // false
        o1 < o2 // false
        let o3 = { 
          name: 1,
          valueOf(){ return this.name }
        }
        let o4 = {
          name: 2,
          valueOf(){ return this.name }
        }
        o4 > o3 // true
      ```
    26. OOP
      ```
        function Rectangle(length, width){
          this.length = length;
          this.width = width;
        }
        Rectangle.prototype.getArea = function(){
          return this.length * this.width;
        }
        Rectangle.prototype.toString = function(){
          return `Rectangle: ${this.length} x ${this.width}`;
        }
        function Square(size){
          Rectangle.call(this, size, size);
        }
        Square.prototype = Object.create(Rectangle.prototype, {
          constructor: {
            value: Square,
            configurable: true,
            enumerable: true,
            writable: true
          }
        })
        Square.prototype.toString = function(){
          let text = Rectangle.prototype.toString.call(this);
          return text.replace('Rectangle', 'Square')
        }
      ```
    27. closure
      ```
        function fn(){
          let private = 1;
          return {
            getPrivate(){
              return private;
            },
            setPrivate(val){
              private = val;
            }
          }
        }
      ```
    28. uber——获取超类
      ```
        function create(superClass, stuff){
          function F(){}
          F.prototype = superClass;
          let n = new F();
          n.uber = superClass;
          Object.keys(stuff).forEach(key => n[key] = stuff[key]);
          return n;
        }
        let shape = {
          name: 'shape',
          toString(){
            return this.name;
          }
        }
        let twoDShape = create(shape, {
          name: '2D shape',
          toString(){
            return this.uber.toString() + ',' + this.name;
          }
        })
        let triangle = create(twoDShape, {
          name: 'triangle',
          side: 0,
          height: 0,
          getArea(){
            return this.side * this.height / 2;
          }
        })
        let m = create(triangle, {
          side: 4,
          height: 10
        })
        m.toString() // return ?
      ```
    29. Module pattern
      ```
        let Person = (function(){
          let age = 23;
          function Inner(name){
            this.name = name;
          }
          Inner.prototype.getAge = function(){
            return age;
          }
          Inner.prototype.ageIncrease = function(){
            age++;
          }
          return Inner;
        })()
      ```
      ```
        function EventTarget(){}
        EventTarget.prototype = {
          constructor: EventTarget,
          addListener(type, listener){
            if(!this._listener){
              this._listener = [];
            }
            if(typeof type === 'undefined'){
              throw new Error('Missing type!');
            }
            if(!this._listener[type]){
              this._listener[type] = [];
            }
            this._listener[type].push(listener);
          },
          fire(ev){
            if(this._listener && Array.isArray(this._listener[ev.type])){
              let listeners = this._listener[ev.type];
              for(let i = 0; i < listeners.length; i++){
                listeners[i].call(this, ev);
              }
            }
          },
          removeListener(type, listener){
            if(this._listener && Array.isArray(this._listener[type])){
              let listeners = this._listener[type];
              for(let i = 0; i < listeners.length; i++){
                if(listeners[i] === listener){
                  listeners.splice(i, 1);
                }
              }
            }
          }
        }
        let target = new EventTarget();
        function test1(ev){
          console.log(ev.msg.toUpperCase());
        }
        function test2(ev){
          console.log(`Hello, ${ev.msg}`);
        }
        target.addListener('test', test1);
        target.addListener('test', test2);
        target.fire({ type: 'test', msg: 'world!' }) // WORLD!, Hello, world!
      ```
    30. mixin实现
      ```
        let obj = {
          name: 'bill',
          get birth(){
            return 1992
          }
        }
        function mixin(target, resource){
          Object.keys(resource).forEach(key => {
            target[key] = resource[key];
          })
        }
        // 缺点: 对于目标元素中包含的访问器属性只是将返回值复制
        function mixin(target, resource){
          Object.keys(resource).forEach(key => {
            let descriptor = Object.getOwnPropertyDescriptor(resouce, key);
            Object.defineProperty(target, key, descriptor);
          })
          return target;
        }
      ```
    31. 安全构造函数
      ```
        function Person(name){
          if(this instanceof Person){
            this.name = name;
          }else{
            return new Person(name);
          }
        }
      ```
    32. 模块化开发——一个js文件就是一个模块
      解决全局变量冲突问题——模块化之前使用命名空间来解决，但是太长的命名空间难以记忆
      按需引入
      文件依赖——模块化之前都是手动引入所需文件，现在可以配合自动化构建工具，自动加载
      组件复用——降低开发成本和维护成本
      组件单独开发，方便分工合作
    33. canvas
      获取绘制上下文——canvas.getContext('2d');
      填充颜色——ctx.fillStyle = 'rgb()';
      绘制图形
        ctx.fillRect(xPos, yPos, width, height);
        ctx.strokeRect(xPos, yPos, width, height);
        ctx.clearRect(xPos, yPos, width, height);
      绘制路径
        beginPath()——绘制路径起点
        closePath()——闭合路径
        stroke()——通过线条绘制图形轮廓
        fill()——填充路径的内容区域(当调用fill时所有未闭合的形状都会自动闭合，所以不需要closePath，但调用stroke时不会自动闭合)
        moveTo(xPos, yPos)——将笔触移动到(xPos, yPos)坐标
        lineTo(xPos, yPos)——绘制一条从当前位置到(xPos, yPos)坐标的直线
        arc(xPos, yPos, radius, startRadians, endRadians, anticlockwise)——画一个以(xPos, yPos)为圆心，radius为半径的圆弧，从startRadians开始到endRadians结束，按照anticlockwise给定的方向(boolean表示顺时针还是逆时针，默认顺时针)来生成
          radians = (Math.PI * 180) / degree
    34. 获取DOM元素属性——window.getComputedStyle(dom, null).getPropertyValue(property)