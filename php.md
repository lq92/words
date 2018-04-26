####双引号和单引号的区别
	1. 双引号解析变量，单引号不解析变量
	2. 在双引号中插入变量，如果变量后有英文或中文要在中间插入分隔符，否则会将整体视为一个变量
	3. 如果双引号中插入变量不想有空格要用大括号包裹变量
	4. 双引号解析转义字符，单引号不解析
	5. 单引号效率高于双引号
	6. 双引号和单引号可以互插
	7. (.)点可以用来拼接字符串
####数据类型: NULL
	1. 初始化变量没有赋值
	2. 赋值为NULL
	3. 使用函数unset()将变量销毁

####empty函数
	传入一个变量，如果变量为NULL、false、空字符串、0则返回true
####isset函数
	传入一组或一个变量，如果有一个是NULL，则返回false		
####gettype函数
	根据传入的变量输出变量类型: integer/double/NULL/string/boolean/array
####var_dump函数
	根据传入的变量输出数据类型和值
		字符串类型会输出长度(一个中文是3)
		数组会输出每一项的数据类型和值		
####判断数据类型
	is_int/is_float/is_bool/is_string/is_array/is_object/is_null/is_resource/is_scalar(是否为标量)/is_numeric(是否为数值类型包含字符串数值)/is_callable(是否为函数)		