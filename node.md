## [nvm](https://github.com/coreybutler/nvm-windows)管理node版本——在windows系统中nvm是一个安装程序，而不是node的一个安装包
    nvm arch——返回当前系统的位数，和安装的node的位数
    nvm install (version) [arch]——安装version版本，和一个可选的位数，默认系统位数，可以是32/64/all
    nvm list/ls (available)——列出安装的版本，加available列出所有可用的
    nvm use (version) [arch]——切换版本
    nvm on——打开node版本管理工具(nvm)
    nvm off——关闭node版本管理工具(nvm)
    nvm uninstall (version)——卸载node版本
## npm install -g无权限问题——提示：npm  wran checkPermission
    删除C:/user/Administrator/AppData/Roaming/npm和npm-cache后重装node
## [supervisor](https://github.com/petruisfan/node-supervisor)自动监控文件修改后自启动服务器
    安装——`npm install supervisor -g`
    `supervisor xxx.js`
## 模块系统
    node通过module.exports/exports暴露模块，通过require加载模块
    require不会重复加载模块，无论调用多少次require获得的模块都是同一个
## 包
    CommenJS规范的包具备以下特征
        package.json要位于根目录下
        二进制文件位于bin目录下
        js文件位于lib目录下
        文档位于doc目录下
        单元测试位于test目录下
    Node在调用一个包时会先检测package.json中的main字段，如果指定该字段会调用这个字段下的文件，如果未指定则调用包下的index.js文件，如果找不到则报错
        name: 包的名称，必须唯一，由小写英文、数字、下划线组成，不能包含空格
        version: 版本字符串
        description: 包的简要说明
        keywords: 包的关键字数组，用于搜索
        maintainers: 包的维护者数组，每个元素包含name、email(可选)、web(可选)字段
        contributors: 贡献者数组，格式与maintainers相同
        bugs: 提交bug的地址，可以是网址或电子邮件地址
        licenses: 许可证数组，每个元素包含type(许可证名称)和url(链接到许可证文本的地址)字段
        repositories: 仓库托管地址数组，每个元素要包含type(仓库的类型，如git)、url(仓库地址)和path(相对于仓库的路径，可选)字段
        dependencies: 包的依赖，一个关联数组，由包的名称和版本号组成
    全局模式安装的包无法通过require获取，可以使用`npm link xxx`将全局的包链接到本地包中后，即可通过require获取
## [断点调试](https://github.com/i5ting/node-debug-tutorial)
    在文件中打断点`debugger;`，然后在命令行中`node debug xxx.js`
## process对象
    ### process.argv——返回一个包含命令行参数的数组，第一项为process.execPath，第二项为执行的js文件路径，剩余元素为执行时传入的额外参数
        ```
            # node index.js name age job
            process.argv.forEach((val, index) => console.log(`${index} : ${val}`))
            /**
              * 0 : C:\Program Files\nodejs\node.exe(node路径)
              * 1 : E:\projects\nodejs\index.js(文件路径)
              * 2 : name
              * 3 : age
              * 4 : job
            */
        ```
    ### process.cwd()——返回当前工作目录的字符串
    ### process.nextTick(callback, [args])——接收一个回调函数和可选的参数，当回调函数执行时，传入的参数作为回调函数的参数
        nextTick中回调函数执行的时机是下一个事件队列开始之前
## util模块
    ### util.inherits(SubConstructor, SuperConstructor)，子类的原型继承父类的原型，现在不建议使用，用es6的extends继承语法，可以通过SubConstructor.super_方法获取SuperConstructor
        ```
            // Sub的实例只继承Super.prototype中的属性/方法
            let util = require('util');
            function Super(name, age){
                this.name = name;
                this.age = age;
                this.sayName = function(){
                    return this.name;
                }
            }
            function Sub(name){
                this.name = name;
            }
            util.inherits(Sub, Super)
            let s = new Sub('Bill') 
        ```
    ### util.inspect(Object, (options))——将对象转换为字符串形式
        options参数
            showHidden: 默认为false，如果为true，则包括不可枚举属性
            depth: 默认为2，表示迭代深度，若为null则表示无限迭代
            colors: 默认为false，若为true，则输出不同类型的数据用颜色标识，自定义颜色：util.inspect.styles(一个对象)
                ```
                    /** util.inspect.styles
                      * 定义的颜色，可以通过util.inspect.colors获取
                    */ 
                    util.inspect.styles = {
                        number: 'blue',
                        string: 'red'
                    }
                ```
## events模块
    events模块只提供了一个对象——EventEmitter
    EventEmitter.on(event, listener)——注册事件
    EventEmitter.emit(event, (args))——触发事件
        ```
            let EventEmitter = require('events').EventEmitter;
            let event = new EventEmitter();
            event.on('test', (arg1, arg2) => console.log(arg1, arg2));
            event.emit('test', 'Bill', 23)
        ```
    EventEmitter.once(event, listener)——注册事件，只调用一次
    EventEmitter.removeListener(event, listener)——移除指定事件的某个监听器，在事件触发后，最后一个监听器完成执行前，任何removeListener()或removeAllListeners()调用都不会从emit()中移除
        ```
            let EventEmitter = require('events').EventEmitter;
            let event = new EventEmitter();
            let callback1 = () => {
                console.log('A');
                event.removeListener('test', callback2);
            };
            let callback2 = () => console.lob('B');
            event.on('test', callback1);
            event.on('test', callback2);
            event.emit('test'); // A B
            event.emit('test'); // A
        ```
    EventEmitter.removeAllListeners((event))——移除所有事件/event事件的所有监听器
## fs模块
    ### 读取文件
        异步: readFile(path, (ecoding), callback)如果不传入ecoding则返回Buffer表示的二进制数据，callback回调函数的参数是(err, data)
        同步: readFileSync(path, (ecoding))
## url模块
    |                                         href                                         |
    ----------------------------------------------------------------------------------------
    | protocol |    |     auth     |        host         |          path          |  hash  |
    |          |    |              |   hostname   | port | pathname |   search    |        | 
    '  https:    //   user : pass  @ sub.host.com : 8080   /p/a/t/h ? query=string   #hash '
    ### url.parse(urlString, (parseQueryString, slashesDenoteHost))——解析url，返回解析后的对象，parseQueryString默认为false，若为true则query属性按照querystring模块的parse方法解析为一个对象，slashesDenoteHost默认为false，若为true则//之后至下一个/之前的字符串会被解析为host，例如: //foo/bar会被解析为{host: 'foo', pathname: '/bar'}
        ```
            let url = require('url');
            let str = 'https://baike.baidu.com/item/CGI/607810?fr=aladdin&fromid=6717913&fromtitle=%EF%BC%A3%EF%BC%A7%EF%BC%A9';
            url.parse(str, true)
            /*{
                protocal: 'https',
                slashes: true,
                auth: null,
                host: 'baike.baidu.com',
                port: null,
                hostname: 'baike.baidu.com',
                hash: null,
                search: '?fr=aladdin&fromid=6717913&fromtitle=%EF%BC%A3%EF%BC%A7%EF%BC%A9',
                query: {fr: 'aladdin', fromid: '6717913', fromtitle: 'ＣＧＩ'},
                pathname: 'item/CGI/607810',
                path: 'item/CGI/607810?fr=aladdin&fromid=6717913&fromtitle=%EF%BC%A3%EF%BC%A7%EF%BC%A9',
                href: 'https://baike.baidu.com/item/CGI/607810?fr=aladdin&fromid=6717913&fromtitle=%EF%BC%A3%EF%BC%A7%EF%BC%A9'
                }*/
        ```
## querystring模块
    ### querystring.parse(str, (sep), (eq), (options))——返回一个对象，用于解析查询字符串
        sep: <string>用于界定查询字符串中的键值对的子字符串，默认为'&'
        eq: <string>用于界定查询字符串中的键与值的子字符串，默认为'='
        options: <object>
            decodeURIComponent: <function>解码查询字符串中的字符时使用的函数，默认为querystring.unescape()
            maxKeys: <number>指定要解析的键的最大数量，指定为0则不限制，默认为1000
        ```
            let querystring = require('querystring');
            let str = 'fr=aladdin&fromid=6717913&fromtitle=ＣＧＩ';
            querystring.parse(str, null, null, { maxKeys: 2 })
            /*
                { 
                    fr: 'aladdin',
                    fromid: '6717913'
                }
             */
        ```
    ### querystring.unescape(str)——对给定的str进行解码，默认使用js内置的decodeURIComponent()进行解码
    ### querystring.stringify(obj, (sep), (eq), (options))——将对象解析成URL查询字符串，如果对象中的属性类型是非Number/String/Boolean/Array类型，则会被强制转换为空字符串
        sep和eq参数与querystring.parse参数一致
        options: <object>
            ecodeURIComponent: <function>把对象中的字符串转换成查询字符串时使用的函数，默认为querystring.escape()
        ```
            let querystring = require('querystring');
            let o = { a: 'a', b: new Date(), c: function(){} };
            querystring.stringify(o, ';', ':'); // 'a:a'
        ```
    ### querystring.escape(str)——将给定的str进行URL编码
## http模块
    http.ServerResponse——是返回给客户端的信息
        ### response.writeHead(statusCode, [headers]): 向请求的客户端发送响应头，statusCode是HTTP状态码，200(成功)、404(未找到)等，headers是一个对象，表示响应头，该函数在一个请求内最多只能调用一次，如果不调用，则自动生成一个响应头
        ### response.write(data, (encoding)): 向请求的客户端发送响应内容，data是一个Buffer或字符串，如果是字符串，则需指定encoding编码方式，默认utf-8，在response.end调用之前可多次调用
        ### response.end((data), (encoding)): 结束响应，该函数必须调用一次，否则客户端将永远处于等待状态
        
