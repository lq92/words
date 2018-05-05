####进入/退出Python交互模式
	打开命令行窗口输入python进入Python交互模式，输入exit()退出Python交互模式或者按ctrl+z然后enter退出

print(): 打印字符串，接收一个或一组字符串，一组字符串中间用逗号分隔，且在解析的时候逗号被解析为空格
执行Python文件: 进入文件所在目录后执行: python xxx.py(文件名只能是英文、数字、下划线组合)	
每一行语句的开始不能有空格或者tab
注释：#开头
###字符串
	####Python有4种类型的数——整数、长整数、浮点数、复数
		1. 整数：2
		2. 长整数：大一些的整数
		3. 浮点数：带有小数点的数
		4. 复数
	####字符串——单引号、双引号、三引号
		1. 单引号字符串''	：引号中制表符空格和tab都原样保留
		2. 双引号字符串""：跟单引号一致
		3. 三引号字符串'''：可以自由的换行，也可以在其内部插入单引号和双引号
		4. 字符串是不可变的
	####转义——\
		例如在单引号字符串中想使用单引号，此时需要对要使用的单引号进行转义
			```
			print('What\'s your name');
			```
		如在一行的末尾加上转义符(\)；则此时表示字符串在下一行继续，而不是开始一个新行(同样适用于三引号字符串)
	####自然字符串——想要指示某些不需要如转义符那样的特别处理的字符串，那么需指定一个自然字符串
		自然字符串通过给字符串加上前缀r或R
		```
		r"this is a string \n" #此时\n不会被转义，即不是一个换行符，如果不加前缀r则是一个换行符
	```		
###变量
	####标识符——以英文(大写或小写)或_开头，后面可以跟英文或者数字或者下划线，标识符对大小写敏感
	声明变量时只需赋值，不需要声明变量类型
###数据类型
	数、字符串和类、Boolean(True, False)		
###运算符
	1. x**y：表示x的y次幂
	2. x//y：表示取商的整数部分
	3. x<<y：表示把x的二进制向左移y位
	4. x>>y：表示把x的二进制向右移y位
	5. x&y：按位与，二进制相同为1，相异为0
	6. x|y：按位或，二进制中只要有1，则为1
	7. x^y：按位异或，二进制中相同为0，相异为1
	8. ~x：按位翻转即-(x+1)
	9. not：布尔非
	10. and：布尔与
	11. or：布尔或	
###控制语句
	1. if控制语句
		```
			if expression1: 
				statement1
				statement2  #optinal
			elif expression2:  #optinal
				statement1
				statement2  #optinal 
			else: 	#optinal
				statement		
		```	
	2. while语句——可选的else语句，当expression为False后执行
		```
			while expression: 
				statement1
			else:  #optional
				statement1	
		```	
	3. for...in循环——迭代队列中的每个项目
		```
			for i in range(1, 5): 
				print(i)
			else: 	# optinal，在for循环语句结束后执行，除非遇到break语句
				print('The loop end!')	
		```	
	***break——终止循环，此时while和for的else语句不会执行***
	***continue——跳出当前循环，继续下一轮循环，else语句执行***