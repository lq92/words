## Webpack
1. 安装: 
	* 局部安装(recommend): npm install webpack + npm install webpack-cli(version 4 or greater) 
	* 全局安装(not recommend/全局安装会锁定webpack版本，当在当前项目中使用不同版本的话，可能会导致错误): npm install webpack -g
2. 打包
	 * 终端执行: node_moduels/.bin/webpack entryName outputName(windows system should use backslashs instead)
	 * 配置webpack.config.js后npx webpack webpack.config.js		