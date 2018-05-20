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
      * 传入eval的代码不能在调用程序所在的上下文中声明变量或定义函数
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
    6. 声明提升——变量和函数声明会提升到执行环境的顶部，在编译阶段发生，只提升声明，赋值操作会留在原地
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
  	


