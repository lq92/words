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
## 对象的扩展
    * 属性/方法简洁表示法——属性名为变量名，属性值为变量值
        let v = 'v', o = { v } // o.v = 'v'
        如果某个方法是generator，则在前面加上*
        let o = {
            * m(){
                yield 'Hello, World!';
            }
        }
        属性名可以使用表达式的值
        let foo = 'a';
        let obj = {
            [foo]: 'b', // a: 'b'
            ['1' + '2']: 'c' // 12: 'c'
        }
        属性表达式如果是一个对象，默认情况下会自动转为字符串[object Object]
        let a = {}, b = {};
        let o = { [a]: '123', [b]: '456' }
        o // { [object Object]: '456' }
    * 方法的name属性
        如果方法是一个set或get方法，则name属性存在该方法的属性描述符上
        let o = {
            get method(){},
            set method(val){}
        }
        let d = Object.getOwnPropertyDescriptor(o, 'method')
        d.set.name // 'set method'
        d.get.name // 'get method'
        如果方法名是一个Symbol，则返回Symbol的描述
        let s1 = Symbol('hello'), s2 = Symbol();
        let o = { [s1](){}, [s2](){} }
        o[s1].name // '[hello]'
        o[s2].name // ''
    * Object.is()——判断两个值是否相等，相当于===，但是NaN等于NaN，-0不等于+0
        Object.defineProperty(Object, 'is', {
            value(x, y){
                if(x === y){
                    return x !== 0 || 1 / x === 1 / y;
                }
                return x !== x && y !== y;
            },
            configurable: true,
            enumerable: false,
            writable: true
        })
    * Object.assign(target, [...source])——将原对象的可枚举属性添加到目标对象上
        继承的属性不拷贝，不可枚举属性不拷贝，Symbol属性也拷贝
        如果源对象有取值函数，则求值后再拷贝
        let source = {
            get foo(){
                return 'foo';
            }
        }
        let o = Object.assign({}, source) // o = { foo: 'foo' }
        拷贝原型链上的属性
        class Person {
            constructor(name){
                Object.assign(this, { name })
            }
        }
        Person.prototype.age = 23;
        let clone = origin => {
            let p = Object.getPrototypeOf(origin);
            return Object.assign({}, p, origin);
            // return Object.assign(Object.create(p), origin);
        }
        let clone = origin => Object.assign(Object.create(Object.getPrototypeOf(origin), Object.getOwnPropertyDescriptors(origin)))
    * 对象的可枚举属性(enumerable)
        如果为false则for...in/JSON.stringify/Object.keys/Object.assign忽略
    * 属性遍历
        for...in——遍历自身和继承的可枚举属性(不包含Symbol)
        Object.keys(obj)——返回自身可枚举属性组成的数组(不包含Symbol)
        Object.getOwnPropertyNames(obj)——返回自身所有属性组成的数组(不包括Symbol)
        Object.getOwnPropertySymbols(obj)——返回自身Symbol属性组成的数组
        Reflect.ownKeys(obj)——返回自身所有的属性组成的数组
    * Object.getOwnPropertyDescriptors(obj)/Object.getOwnPropertyDescriptor(obj, property)——返回对象的属性描述符对象
        let mixin = obj => ({
            with: (...mixins) => mixins.reduce((prev, next) => Object.create(prev, Object.getOwnPropertyDescriptors(next)), obj)
        })
    * 设置原型——Object.setPrototypeOf(child, parent)
    * 获取原型——Object.getPrototypeOf(obj)
    * 指向原型——super
        let o = { foo: 'hello' }, o1 = { foo: 'world', f(){ return super.foo }};
        Object.setPrototypeOf(o1, o);
        o1.f() // return 'hello'，必须这样定义super
        let parent = { name: 'bill', getName(){ return this.name; } };
        let child = { name: 'gate', getName(){ return super.getName() }};
        Object.setPrototypeOf(child, parent);
        child.getName(); // return 'gate'
    * Object.keys(obj)/Object.values(obj)/Object.entries(obj)
    * 对象的扩展运算符和解构赋值
        let o = { school: 'middle' };
        Object.defineProperties(o, {
            name: {
                value: 'bill',
                enumerable: true
            },
            age: {
                value: 23
            }
        })
        let { name, t, ...test} = o
        // name = 'bill'，t = undefined，test = { school: 'middle' }
        解构赋值是浅拷贝，不复制继承的值
        let o = Object.create({ x: 1, y: 2 });
        o.z = 3;
        let {x, ...obj} = o;
        // x = 1，obj = { z: 3 }
        如果使用解构赋值，则扩展运算符(...)后必须是一个变量名，而不能是一个解构赋值表达式
        let {x, ...{y, z}} = o // 报错
## Symbol
    * 数据类型
        undefined/null/Number/String/Boolean/Object/Symbol
        typeof返回'undefined'/'number'/'string'/'boolean'/'object'/'symbol'
    * Symbol是一种基本数据类型Symbol('test')接收一个描述性字符串
    * Symbol值不能与其他类型的值进行运算，会报错
        'String ' + Symbol('test') // TypeError
    * 将Symbol转为字符串——String()/Symbol().toString()
    * Symbol可以转换为布尔值，不能转为数值
        Boolean(Symbol()) // true
    * Symbol.for(str)——该方法会先在全局范围内查找是否有以str作为名称的值，如果有则返回，否则新建一个，生成的值在全局范围内登记，Symbol(str)则不会在全局内登记
        let s1 = Symbol.for('str'), s2 = Symbol.for('str') // s1 === s2
        let s1 = Symbol('str'), s2 = Symbol.for('str') // s1 !== s2
        Symbol(str)方法没有登记机制，每次都返回不同的Symbol
    * Symbol.kefFor(symbol)——返回一个已登记的symbol的key
        let s1 = Symbol.for('test'), s2 = Symbol('test');
        Symbol.keyFor(s1) // 'test'
        Symbol.keyFor(s2) // undefined
## Set和Map
    * Set的数据结构类似数组，但不包括重复的值(NaN与NaN相等，其余情况按照===判断)，构造函数接收一个数组或部署了Iterator，可以使用链式调用
        function* fib(n){
            let [a, b] = [0, 1];
            let num = 0;
            while(num < n){
                yield b;
                [a, b] = [b, a + b];
                num++;
            }
        }
        let s = new Set(fib(10))
        // s = [1, 2, 3, 5, 8, 13, 21, 34, 55]
    * Set实例的属性
        constructor指向Set构造函数
        size表示实例对象的成员总数
    * Set实例方法
        add()添加成员
        delete()删除成员
        has()判断是否有成员
        clear()清空对象
    * Set的遍历
        keys()/values()/entries()/forEach()
        keys()——返回键和values()返回的结果一致
        entries()——返回键值对
        keys/values/entries返回的都是Iterator对象
        forEach与数组的forEach一致
        应用：
            去除重复数组
                let arr = [];
                let { random, floor } = Math;
                let i = 0;
                while(i < 20){
                    arr.push(floor(random() * 20));
                    i++;
                }
                [...new Set(arr)]
                Array.from(new Set(arr))
            将Set转化为数组后就可以用数组的所有方法了
                let s = new Set([1, 2, 3, 4, 5, 6, 7, 8]);
                new Set(Array.from(s, x => x + 2))
                new Set([...set].filter(x => x % 2 === 0))
            求两个Set的交集、并集、差集
                let s1 = new Set([1, 2, 3, 4]),
                    s2 = new Set([2, 3, 5]);
                new Set([...s1, ...s2])
                new Set([...s1].filter(x => s2.has(x)))
    * Map数据结构——表示对象，可以使用任意数据类型作为键，Map构造函数可以接收一个数组作为参数，该数组的成员是表示键值对的数组，如果对同一个键多次赋值，后面的会覆盖前面的
        let s = new Set([[1, 2], [3, 4], [1, 2]])
        let m = new Map(s)
        s.size // 3
        m.size // 2
    * Map的属性和方法
        size
        set(key, val)
        get(key)
        delete(key)
        has(key)
        clear()
    * Map的遍历
        keys()
        values()
        entries()
        forEach()
        let m = new Map(['name', 'bill'], ['age', 23], ['score', 98], ['school', 'middle']);
        m.forEach((key, val) => console.log('Key: %s, Value: %s', key, val))
        Map转对象
        let mapToObj = map => {
            let obj = {};
            for(let [key, val] of map){
                obj[key] = val;
            }
            return obj;
        }
        对象转Map
        let objToMap = obj => {
            let map = new Map();
            for(let [key, val] of Object.entries(obj)){
                map.set(key, val);
            }
            return map;
        }
        Map转为JSON
        let mapToJson = map => JSON.stringify(mapToObj(map))
        JSON转为Map
        let jsonToMap = json => objToMap(JSON.parse(json))
## Proxy
## Reflect
## Promise对象
    异步编程的解决方案，Promise有两个特点：1、对象的状态不受外界影响，只有异步操作的结果决定当前是哪种状态(pending/fulfilled/rejected)；2、一旦状态改变，就不会再改变，任何时候都会得到这个结果
    let promise = new Promise((resolve, reject) => {
        if(/* 异步操作成功 */){
            resolve(value) // 异步操作成功时调用，并将结果作为参数传递出去 
        }else{
            resolve(error) // 异步操作失败时调用
        }
    })
    promise.then(value => {
        // resolved状态的回调函数
    }, error => {
        // rejected状态的回调函数
    })
    Promise新建后会立即执行
    let p = function(){
        new Promise((resolve, reject) => {
            console.log('promise1');
            resolve('success');
        }).then(val => console.log(val))
        return new Promise((resolve, reject) => {
            console.log('promise2');
        })
    }
    p()() // 输出：promise1/promise2/success
    异步加载图片
    let loadImg = url => {
        return new Promise((resolve, reject) => {
            let img = new Image();
            img.src = url;
            img.addEventListener('load', function(){
                resolve(this);
            })
            img.addEventListener('error', () => reject(new Error(`The ${url} isn't correct`)))
        })
    }
    loadImg('xxx').then(dom => document.body.appendChild(dom))