## npm笔记
1. 更新npm版本: npm install npm@latest -g
2. 使用下一个未发布的版本：npm install npm@next -g
3. 初始化package.json文件: npm init / npm init --yes(-y)
		## 设置init默认的信息
		* npm set init.author.email "your_email"
		* npm set init.author.name "your_name"
		* npm set init.license "MIT"
4. npm安装包npm install <package_name> --save(生产环境) !default / npm install <package_name> --save-dev(开发环境)		
5. npm更新包: npm update
6. npm卸载包: npm uninstall <package_name> 
7. 全局哪些包的版本需要更新/过时: npm outdated -g --depth=0
8. 更新全局包: npm update -g <package_name>
9. 卸载全局包: npm uninstall -g <package_name>
10. CLI添加注册账户: npm adduser
11. CLI登录账户: npm login
12. CLI是否登录: npm whoami
13. 查看主页: https://npmjs.com/~yourname 
14. 发布包: npm publish
15. 更新包: npm version <update_type> + npm publish
16. 更新README.md: npm version patch + npm publish 
17. npm包的版本标准：
		* 第一次发布版本号：1.0.0
		* fix bugs and patch: 递增第三位
		* 增加新特性/对之前版本没有破坏: 递增第二位
		* 打破之前版本的兼容性: 递增第一位



///////////////////////////
### optional
1. Add tags: npm dist-tag add <package>@<version> [<tag>]
2. Publish with tags: npm publish --tag <tag>
3. Install with tags: npm install package@<tag>	
