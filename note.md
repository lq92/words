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
    7. Cookie/sessionStorage/localStorage
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
  	(https://www.cnblogs.com/cencenyue/p/7604651.html)


