## Webpack(https://webpack.js.org/guides)
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
7. 多入口文件分开打包
	* 配置webpack.config.js
	```
	module.exports = {
		entry: {
			alias1: 'pathname1',
			alias2: 'pathname2'
		},
		output: {
			filename: '[name].js',
			path: path.resolve('__dirname', 'dist')
		}
	}
	```
8. html-webpack-plugin: 入口文件打包后更改名字，使用这个插件会自动更改，这个本质是重新生成html文件去替换项目文件[github](https://github.com/jantimon/html-webpack-plugin)
	```
	const HtmlWebpackPlugin = require('html-webpack-plugin')
	module.exports = {
		plugins: [
			new HtmlWebpackPlugin({
				title: ''
			})
		]
	}
	```
9. clean-webpack-plugin: 清空文件目录[github](https://github.com/johnagan/clean-webpack-plugin)
	```
	const CleanWebpackPlugin = require('clean-webpack-plugin')
	module.exports = {
		plugins: [
			new CleanWebpackPlugin(['dirname'])
		]
	}
	```
10. 开发环境错误跟踪工具: devtool / [docs](https://webpack.js.org/configuration/devtool/)
	```
	module.exports = {
		devtool: 'inline-source-map'
	}
	```	
11. 自动打包
	1. watch模式: 缺点-需要手动刷新浏览器查看
		* package.json中配置scripts
		```
		scripts: {
			"watch": "webpack --watch"
		}
		```	
<<<<<<< HEAD
	2. 本地开启服务器模式(无需手动刷新浏览器,默认打开8080端口): webpack-dev-server / [docs](https://webpack.js.org/configuration/dev-server/)
		* 安装webpack-dev-server
		```
		npm install webpack-dev-server -D
=======
	2. 本地开启服务器模式(无需手动刷新浏览器): webpack-dev-server / [docs](https://webpack.js.org/configuration/dev-server/)
		* 安装webpack-dev-server
		```
		npm install webpack-dev-server
		```
		* 配置webpack.config.js
		```
		module.exports = {
			devServer: {
				contentBase: './dist'
			}
		}
		```
		* 配置package.json中scripts
		```
		scripts: {
			"start": "webpack-dev-server --open"
		}
		```	
		* npm start
<<<<<<< HEAD
	3. 使用中间件(会自动编译，但是不能自动刷新): webpack-dev-middleware(内部也还是调用webpack-dev-server,但是可以自定义设置) / [docs](https://webpack.js.org/guides/hot-module-replacement/)
=======
	3. 使用中间件(会自动编译，但是不能自动刷新): webpack-dev-middleware(内部也还是调用webpack-dev-server,但是可以自定义的设置) / [docs](https://webpack.js.org/guides/hot-module-replacement/)
>>>>>>> 7c94a45a0b04ecb8fdbee032df52e1078c705476
	使用express和webpack-dev-middleware	
		* 安装express和webpack-dev-middleware
		```
		npm install express webpack-dev-middleware -D
		```
		* 配置webpack.config.js
		```
		module.exports = {
			output: {
				publicPath: '/'
			}
		}
		```
		* 配置package.json
		```
		"scripts": {
			"server": "node server.js"
		}
		```
		* 新建server.js开启本地服务器
		```
		const express = require('express')
		const webpack = require('webpack')
		const WebpackDevMiddleware = require('webpack-dev-middleware')

		const app = express()
		const config = require('./webpack.config.js')
		const compiler = webpack(config)

		app.use(WebpackDevMiddleware(compiler, {
			publicPath: config.output.publicPath
		}))

		app.listen(port, () => {
			console.log(`The server is running at ${port}`)
		})
		```
12.	***Hot Module Replacement***
13. sideEffects: 将引入模块中未使用的代码不打包进最终文件(根据官方文档中配置在package.json中配置***未实现***)
	```
	{
		"sideEffects": false
	}
	```	
	```
	{
		"sideEffects": [
			"relative/absolute"
		]
	}
	```
	* 可以在webpack.config.js中配置成生产模式已压缩打包文件大小
	```
	{
		mode: 'production'
	}
	```
14. 根据环境不同配置不同的webpack[详见](https://github.com/lq92/init-project.git)	

