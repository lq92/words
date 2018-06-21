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
    安装——npm install supervisor -g
    supervisor xxx.js
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
        name: 包的名称，必须唯一，由小写英文、数组、下划线组成，不能包含空格
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

