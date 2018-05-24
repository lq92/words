## let和const
    * let声明的变量是在块级作用域的，外部访问不到
    * for循环中设置循环变量的是父作用域中的，循环体内部是子作用域
        ```
            for(let i = 0; i < 3; i++){
                let i = 'ab';
                console.log(i) // log 'ab'
            }
        ```
    * 声明的变量不存在变量提升
    * 暂时性死区——只要块级作用域中存在let或const声明的语句，则存在死区，即在声明之前操作变量会报错(ReferenceError: variable is not defined)
        死区导致typeof操作符变的不安全
            ```
                typeof x; // ReferenceError
                let x;
                typeof y // return 'undefined'
            ```
        死区会导致函数设置默认值时
            ```
                function foo(x = y, y = 2){
                    return x + y;
                }
                foo() // ReferenceError,将y赋值给x之前，y未定义，死区
            ```
    * 当前作用域中不能重复声明，函数体内不允许声明跟形参同名变量
        ```
            function foo(x){
                let x;
            }
            foo(2) // SyntaxError
        ```
    * const表示指向变量的内存地址不变，必须初始化
        ```
            const obj = { a: 1, b: [2, 3] }
            obj.a = 2 
        ```
    * let和const声明的变量不是全局对象的属性
        ```
            var a = 1;
            let b = 2;
            window.a // 1
            window.b // undefined
        ```
## 变量的解构赋值
    * 数组解构赋值——只要等号左右的模式相同即可赋值，数组元素按次序排列
        let [a, b, c] = ['x', 'y', 'z']
        如果等号右侧不是一个Iterable则报错
            let [a, b, c] = 20 // TypeError
        解构不成功变量的值为undefined
        默认值——使用===判断值是否是undefined
            let [a = 1] = [] // a = 1 表示如果a没有解构赋值则为1
            let [a = 2] = [null] // a = null
        默认值也可以解构赋值
            let [x = 1, y = x] = [2, 1] // x = 2, y = 1
    * 对象解构赋值——变量名和对象名相同
        let { bar, baz } = { bar: 1, foo: 2 } // bar = 1, baz = undefined
        let { foo: baz } = { foo: 2 } // baz = 2, foo not defined
        let obj = {
            a: {
                b: 1,
                c: [2, 3]
            }
        }
        let { a: {b, c} } = obj // b = 1, c = [2, 3], a not defined, 浅复制
        默认值——生效的条件是对象的属性值严格等于undefined
            let { x: y = 3 } = { y = 5 } // y = 5
        解构嵌套对象，若父对象不存在则报错(因为此时父对象是undefined)
            let { foo: { bar } } = { bar: 3 } // TypeError
        将一个已声明的变量用于解构，要加括号
            let obj = {};
            ({ obj } = { obj: { foo: 3 }}) // obj = { foo: 3 }
        数组也是特殊的对象，因此数字也可以当做被解构的对象
            let arr = [2, 3, 4];
            let { 0: num1, 1: num2, [arr.length - 1]: num3 } = arr // num1 = 2, num2 = 3, num3 = 4
    * 字符串的解构赋值
        let [a, b, c, d, e] = 'Hello' // a = 'H', b = 'e', c = 'l', d = 'l', e = 'o' 字符串是类数组，可以使用数组解构赋值
        let { length: len } = 'Hello' // len = 5
    * 等号右侧是null或undefined时，解构赋值会报错，为boolean或number时，先转换为对象再解构
    * 函数参数的解构赋值
        function foo({x, y}){
            return [x, y];
        }
        foo({ x: 2, y: 3 }) // return [2, 3]
        function foo([x, y] = [1, 2]){
            return [x, y];
        }
        foo() // return [1, 2]
        foo([2, 3]) // return [2, 3]
        function foo({x = 0, y = 0}){
            return [x, y];
        }
        foo() // TypeError
        foo({}) // return [0, 0]
        foo({ x: 2, y: 4 }) // return [2, 4]
        foo({ x: 3 }) // return [3, 0]
        function foo({x = 0, y = 0} = {}){
            return [x, y];
        }
        foo() // return [0, 0]
        foo({ x: 3 }) // return [3, 0]
## 函数的扩展
    * 函数参数默认值若是一个表达式的话，每次会重新计算表达式的值，是惰性求值
        let x = 10;
        function foo(y = x + 1){
            return y;
        }
        foo() // 11
        foo() // 11
    * 函数的默认参数一般放在参数末尾，否则除了赋值为undefined，无法省略
        function foo(x = 2, y){
            return [x, y]
        }
        foo(, 3) // SyntaxError
        foo(undefined, 3) // return [2, 3]
    * length属性——表示没有指定默认值的参数个数，如果默认值放在中间，则默认值之后的参数也不计入，...rest参数也不计入
        (function(x, y = 1, z){}).length // return 1
    * 当函数参数设置默认值时，此时函数的参数形成了一个独立的作用域
        var x = 1;
        function foo(x, y = () => x = 2){
            var x = 3;
            y();
            console.log(x) // return 3
        }
        function bar(x, y = () => x = 2){
            x = 3;
            y();
            console.log(x) // return 2
        }
        function baz(x, y = () => x = 2){
            y();
            x = 3;
            console.log(x) // return 3
        }
    * rest参数——...variableName，用来获取函数的参数，是一个数组，rest只能是最后一个参数
    * name属性
        将匿名函数赋值给一个变量，则name为变量名
        将具名函数赋值给一个变量，则name为具名函数原本的名字
            let f = function bar(){}; f.name // return 'bar'
        Function构造函数返回的实例，name为anonymous
        bind返回的函数，name属性为：bound functionName
        (function(){}).name，返回的是''
    * 箭头函数
        箭头函数要是返回一个对象的话，要加上()，否则报错
            let f = () => ({ a: 1, b: 2 })
        注意点：
            箭头函数的this是定义时指定
            函数体内不能使用arguments对象，可以用...rest
            不能用作构造函数
            函数体内不能使用yield
        向数组中插入值
        let insert = value => ({ into: arr => ({ after: afterValue => {
            arr.splice(arr.indexOf(afterValue) + 1, 0, value);
            return arr;
        }})})
        管道
        let pipe = (...funcs) => val => funcs.reduce((prev, next) => next(prev), val);
        let plus = val => val + val;
        let subtract = val => val - 2;
        let multi = val => val * val;
        let divide = val => val / 2;
        let f = pipe(plus, subtract, multi, divide)
        f(20) // return 722
    * 尾调用——函数的最后一步是调用另一个函数
        优化：防止堆栈溢出
        function outFact(n, total){
            if(n === 1) return total;
            return outFact(n - 1, n * total)
        }
        function fact(n){
            return outFact(n, 1)
        }
        function fib(n, a = 1, b = a){
            if(n <= 1) return b;
            return fib(n - 1, b, a + b)
        }
## 数组的扩展
    * 扩展运算符...——将数组展开，相当于函数...rest的逆运算
        [1, 2, 3].push(...[4, 5, 6])
        复制数组
            let copy = [1, 2, 3].concat();
            let copy = [...[1, 2, 3]];
            let [...copy] = [1, 2, 3]
        合并数组
            let a = [1, 2, 3], b = [4, 5, 6], c = [7, 8, 9];
            let d = a.concat(b, c)
            let d = [...a, ...b, ...c]
        与解构赋值结合——必须放在最后，否则报错
            let [a, ...arr] = [1, 2, 3, 4] // a = 1, arr = [2, 3, 4]
        实现了Iterator即可用扩展运算符
            [...'hello'] // ['h', 'e', 'l', 'l', 'o']
            function* g(){ yield 1; yield 2; yield 3;}
            [...g()] // [1, 2, 3]
    * Array.from——将类数组对象和部署了Iterator的对象转换为数组
        类数组对象——只要有length属性的对象
            let o = { a: 1, b: 2, length: 3 } Array.from(o) // return [undefined, undefined, undefined]
        还可以接收一个函数参数，作用和map的函数类似，将数组项作用于函数后返回
            Array.from([1, 2, 3], x => Math.pow(x, 3)) // return [1, 8, 27]
        判断函数参数的数据类型
            function type(){
                return Array.from(arguments, x => typeof x);
            }
            function type(...arg){
                return arg.map(item => typeof item);
            }
        还可以接收指向this值的参数
            let obj = { a: 1, b: 2, c: 3 };
            Array.from(['a', 'b', 'c'], function(item){
                return this[item]; // return [1, 2, 3]
            }, obj)
    * Array.of()——将一组值转换为数组，弥补Array的不足
        Array.of(3) // return [3]
        Array(3) // return [undefined * 3]
    * copyWithin(target, start, end)——从start到end复制到target上，返回原数组
    * find()和findIndex()——一个返回数组项，一个返回下标，都接收一个函数和指向this的obj作为参数，find未找到返回undefined，findIndex未找到返回-1，回调函数可以接收三个参数(item, index, arr)
    * fill(item, start, end)——从start到end填充item，如果填充的是对象，则填充地址
    * entries()/keys()/values()——用于遍历数组，返回Iterator
        let arr = ['a', 'b', 'c', 'd'];
        for(let k of arr.keys()){ console.log(k) } // return 0, 1, 2, 3
        for(let v of arr.values()){ console.log(v) } // return 'a', 'b', 'c', 'd'
        for(let [k, v] of arr.entries()){ console.log(k, v) } // return [0, 'a'], [1, 'b'], [2, 'c'], [3, 'd']
    * includes(val, start)——判断数组中从start起是否有val，NaN
        Array.prototype.includes = Array.prototype.includes || function(val, start){
            return this.indexOf(val, start) !== -1;
        }