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



