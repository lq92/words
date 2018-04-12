## Webpack
1. 安装: 
	* 局部安装(recommend): npm install webpack + npm install webpack-cli(version 4 or greater) 
	* 全局安装(not recommend/全局安装会锁定webpack版本，当在当前项目中使用不同版本的话，可能会导致错误): npm install webpack -g
2. 打包流程
	* npm init -y(初始化package.json文件)
	* npm install webpack webpack-cli --save-dev(安装webpack包)
	* npm install lodash(安装lodash模块)
	* src文件夹放项目源文件、dist文件夹放打包后的文件
	* 配置webpack.config.js文件
	* 执行打包: npx webpack --config webpack.config.js(alias: 在package.json文件中scripts设置打包别名后可以通过npm run alias完成打包，可以添加自定义参数: npm run alias -- --parameters) 	
3. 加载style
	* 安装style-loader和css-loader: npm install style-loader css-loader -D
	* 修改webpack.config.js
		```
		module: {
			rules: [
				{
					test: /\.css$/,
					use: [
						'style-loader',
						'css-loader'
					]
				}
			]
		}
		```
	* 在入口文件中通过import引入样式表	
4. 加载图片
	* 安装file-loader: npm install file-loader -D
	* 配置webpack.config.js
	```
	module: {
		rules: [
			{
				test: /\.(png|jpg|gif|svg)$/,
				use: [ 'file-loader' ]
			}
		]
	}
	```	
	* 在样式表中引入background或者在js中使用图片，都可以正确解析。webpack会将引入的图片打包到目标文件中
5. 加载字体
	* 安装file-loader: npm install file-loader -D
	* 配置webpack.config.js
	```
	module: {
		rules: [
			{
				test: /\.(svg|woff|ttf)/,
				use: [ 'file-loader' ]
			}
		]
	}
	```	
6. 加载xml文件、cvs文件和tsv文件
	* 安装cvs-loader和xml-loader: npm install xml-loader cvs-loader -D
	* 配置webpack.config.js
	```
	module: {
		rules: [
			{
				test: /\.xml$/,
				use: [ 'xml-loader' ]
			},
			{
				test: /\.(cvs|tsv)$/,
				use: [ 'cvs-loader' ]
			}
		]
	}
	```	
	* 引入文件到项目入口文件: import variable form 'dirname'
