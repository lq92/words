### 双引号和单引号的区别
	1. 双引号解析变量，单引号不解析变量
	2. 在双引号中插入变量，如果变量后有英文或中文要在中间插入分隔符，否则会将整体视为一个变量
	3. 如果双引号中插入变量不想有空格要用大括号包裹变量
	4. 双引号解析转义字符，单引号不解析
	5. 单引号效率高于双引号
	6. 双引号和单引号可以互插
	7. (.)点可以用来拼接字符串
### 数据类型: NULL
	1. 初始化变量没有赋值
	2. 赋值为NULL
	3. 使用函数unset()将变量销毁

### empty函数
	传入一个变量，如果变量为NULL、false、空字符串、0则返回true
### isset函数
	传入一组或一个变量，如果有一个是NULL，则返回false		
### gettype函数
	根据传入的变量输出变量类型: integer/double/NULL/string/boolean/array/object
### var_dump函数
	根据传入的变量输出数据类型和值
		字符串类型会输出长度
		数组会输出每一项的数据类型和值		
### 判断数据类型
	is_int/is_float/is_bool/is_string/is_array/is_object/is_null/is_resource/is_scalar(是否为标量integer/float/string/boolean)/is_numeric(是否为数值类型包含字符串数值)/is_callable(是否为函数)	
### 布尔值的自动类型转换
	1. NULL为假
	2. 空字符串为假，有且仅有一个0的字符串为假，其余为真
	3. 0为假，0.0为假，其余为真
	4. 空数组为假
	5. 未声明成功的资源为假
### 类型转换
	1. true和false在参与运算时会自动转换成1和0
	2. 字符串前的整数或者浮点数在参与运算时会将开始处的数字参与运算
### 强制类型转换
	1. 运用三个转型函数：intval()/floatval()/strval()
		* intval($variable, [base])：默认基数是10，空数组返回0，非空数组返回1
	2. 在变量前加上()括号类型，不改变原变量
	3. settype(变量, 类型)直接改变变量本身	
### 常量
	1. 定义常量define(常量名, 常量值)
	2. 在字符串中引入常量需放在引号之外用.点连接符
### 可变变量
	可变变量即在变量前再加上$		
### 外部变量
	外部变量是php中规定好的变量
	1. $_GET: 处理get请求，会将请求信息放在url中传输给后台，这样信息无法受到保护
	2. $_POST: 处理post请求，请求的信息会放在请求头中，所以在浏览器中是看不到的
	3. $_REQUEST：既能处理get请求也能处理post请求
	4. $_FILES: 得到文件的上传结果
	5. $_SESSION: 得到会话控制中session的值
	6. $_COOKIE: 得到会话控制中cookie传值	
### 变量引用
	1. 将一个变量赋值给另一个变量时，两个变量只是值相等，引用的地址不等，此时改变一个变量，另一个变量不变
	2. 在一个变量前加上&符号再赋值给一个变量，此时两个变量是指向同一个地址，此时改变一个变量，另一个变量也跟着变
### 自增/自减
	前置和后置之分：++$x/$x++/--$x/$x--
	前置是先赋值后自增/自减；后置是先自增/自减后赋值
### 逻辑操作符
	1. 逻辑与 $x and $y / $x && $y
	2. 逻辑或 $x or $y / $x || $y
	3. 逻辑非 !$x
	4.逻辑异或 $x xor $y相同取反，相异取正			
### 循环语句
	1. while循环
	2. do...while循环
	3. switch循环
	4. for循环	
		break跳出循环并终止；continue跳出当前循环执行下一次循环；exit退出程序；
		```
			$i = 1;
			while(true){
				if($i === 2){
					// $i++; 这加与不加有什么区别;
					continue;
				}else if($i === 5){
					break;
				}else{
					echo $i . '<br/>';
				}
				$i++;
			}
		```
	5. goto跳出循环：在当前文件或作用域中goto后加上跳出标记，即可转到目标位置的标记
		```
			$i = 0;
			while($i < 10){
				echo $i . '<br />';
				if($i === 5){
					goto end;
				}
				$i++;
			}
			end: 
			echo '到' . $i . '结束了';
		```
### 函数function
	1. 函数名不区分大小写，首字母不用$
	2. 函数的执行和定义的顺序没有关系
	3. 函数传参可以设置默认值，形参和默认值之间用=等号赋值
		```
			function fn($var1, $var2 = 'hello'){
				echo $var2;
			}
			fn(2) // $var1 === 2;
		```		
	4. 函数定义时传入形参在执行时必须赋值，否则报错，上面的fn函数如果调用时不传入var1的值会报错
	5. 传入默认值的形参可以在调用时不传值
	6. 函数的无默认形参和默认形参在定义时一般把无默认形参放前，否则调用时要传入值
	```
		function fn2($var1, $var2 = 3, $var3){
			return $var2;
		}
		fn2(2, ,3) // 报错
	```	
	7. 函数的实参传值是按值传递，而不是按引用传递
	8. 函数不能重载，即同一个作用域中不能定义相同的函数名的函数，会报错
	9. 回调函数：把一个函数的函数名当字符串传入另一个函数内执行，就形成了回调函数
	10. 可变函数：把一个函数名（可以加引号或者不加引号）赋值给一个变量，即把这个函数的引用赋值给这个变量，这样就可以用变量名()（加括号）执行函数体
	11. 函数内部函数：即在一个函数内再声明一个函数，内部函数需外部调用后才定义，内部函数不能重复定义，外部函数也只能调用一次，否则会报错，内部函数不是外部函数的作用域内
	12. 函数体内定义的变量只能在函数内使用，若想在函数体外使用则需要定义为超全局变量的项数($GLOBALS是一个数组)
		```
			function fn(){
				$GLOBALS['a'] = 10;
			}
			fn();
			echo $a; // 10
		```
	13. 在全局中定义的变量就相当于在$GLOBALS上定义项数
		```
			$str = 'hello'; 
			echo $GLOBALS['str']; // 'hello'
		```
	14. 函数体内要调用或修改体外的变量需通过$GLOBALS['variableName']来调用
		```
			$a = 10;
			function fn(){
				$result = 10 + $GLOBALS['a'];
				echo $result;
			}
			fn(); // 20
		```
	15. 可以通过在函数体内用$GLOBALS['variableName']来定义全局变量
	16. 可以在函数体内通过global $variable_one $variable_two;来引用过全局的变量
		```
			$a = 10; 
			$b = 20;
			function fn(){
				global $a, $b;
				echo $a + $b;
			}
			fn(); // 30
		```
	17. 函数参数的变量引用：在形参变量前加上&表明形参和实参都是指向同一个地址的引用
	18. 函数体内定义的变量要赋值
### 静态变量
	static $variable_name静态变量的特点是多次调用函数不会重置该变量
### 内置函数
	* copy(): 接收两个字符串参数，第一个字符串表示源文件，第二个参数表示目标文件，该函数的功能是将源文件的内容拷贝到目标文件中，如果拷贝成功则返回true，否则返回false
	* array_unshift(): 接收一个数组(目标数组)和一个或一组数组项，并将其插入到目标数组中 
### 包含外部文件
	* require: 请求外部文件，如果遇到错误则停止执行
	* require_once: 请求外部文件，会做一次是否是第一次请求检查，如果已请求过则此次略过
	* include: 请求外部文件，如果遇到错误会报错，但会继续执行下面的代码
	* include_once: 请求外部文件，会做一次是否是第一次请求检查，如果已请求过则此次略过
### 常用数学函数
	* max(arr|...parameters): 计算出一个数组或一系列数的最大数
	* min(arr|...parameters): 计算出一个数组或一系列数的最小数
	* abs(): 计算绝对值
	* rand($min, $max): 随机产生最大值和最小值之间的整数
	* mt_rand($min, $max): 更好的随机产生最大值和最小值之间的整数
	* pi(): 圆周率
	* ceil(): 向上取整
	* floor(): 向下取整	
	* pow($x, $y): 返回$x的$y次幂
	* round(): 浮点数四舍五入，默认返回一个整数，可接收一个表示保留几位数的参数
	* sqrt($x): 返回$x的平方根
	* fmod(): 浮点数取模
### 时间
	1. 时区
		设置时区: date_default_timezone_set('Asia/Shanghai')
		获取时区: date_default_timezone_get()
	2. 世界时(格林尼治地方时间)
	3. unix时间戳(自1970年1月1日零时起到现在的毫秒数)	
		time()获取时间戳
		date('Y m d H i s')获取本地时间，可选参数时间戳
		getdate()获取当前系统时间，返回一个数组
			seconds--------秒
			minutes--------分
			hours----------时
			mday-----------月份中第几天	
			wday-----------数字周几(0-6)
			mon------------数字月份
			year-----------年份
			yday-----------一年中的第几天
			weekday--------英文周几
			month----------英文月份
			0--------------时间戳
		checkdate(month, day, year): 检验输入的日期是否正确，返回boolean，参数为月日年	
		mktime(hours, minutes, seconds, month, day, year): 生成时间，返回时间戳
		strtotime(string): 返回日期的时间戳
		microtime(true): 返回unix时间戳的微秒数
### 字符串方法
	1. trim(string): 删除字符串前后空格
		ltrim(string)
		rtrim(string)/chop(string)
	2. str_pad(string, str_num, [填充字符])
	3. str_repeat(string, num): 重复字符串次数
	4. str_split(string): 将字符串分隔成数组
	5. strrev(string): 反转字符串
	6. wordwrap(string, num): 将字符串按照指定num分隔换行
	7. strlen(string): 统计字符串长度
	***汉字在GBK编码中是双字节，在UTF-8中是三字节***	
### 数组
	1. 数组定义(数组中可以定义任意类型的数据)
		* 索引值(索引可以不用从0开始)
			```
				$arr = array(1, 1.2, false, 'str')
				or
				$arr = array(
					0 => 1,
					1 => 1.2,
					2 => false,
					3 => 'str'
				)
				or
				$arr = array(
					0 => 1,
					4 => 1.2,
					false,
					1000 => 'str'
				)
			```	
			1. 索引数组要是不指定下标，那下标是从0开始
			2. 如果指定下标，那下标是指定的值
			3. 若指定下标，后一个下标没有指定的话，那后一个下标在前一个的基础上加1
		* 数组操作
			1. 增加数组元素
				$arr[] = xxx(此时的元素索引是最大数字索引加1)	
			2. 修改元素
				$arr[index] = xxx
			3. 删除数组元素
				unset($arr[index])	
				删除了值，并不会让后面的下标向前移动，而是原来的值为多少就是多少
				删除某个值，新加入的值不会替换掉原来的位置
		* 数组字面量
			```
				$arr = [
					1, 
					20 => 1.2,
					false,
					30 => 'str'
				]
			```		
		* 关联数组(带有指定键的数组)
		```
			$arr = [
				'str' => 'strrr',
				'str1' => 'str111'
			]
		```	
		* 数组长度: count($arr)
		* 数组遍历
			1. 连续的索引数组遍历：循环遍历
				```
					$arr = array(1, 2, 3, 4, 5, 6, 7, 8);
					for($i = 0; $i < count($arr); $i++){
						echo $arr[$i] . '<br />';
					}
				```
			2. 不连续的索引数组和关联数组遍历: foreach遍历
				```
					$arr = [
						'str1' => 'str11',
						'str2' => 'str22'
					]
					foreach($arr as $key => $val){ // $key optional
						echo $key . '-----' . $val;
					}
				```
			3. list(variable1, variable2...)函数遍历: 遍历的是索引数组，且接收变量与数组是一一对应关系，第一个变量对应的是数组索引值为0的值
				```
					list($one, $two) = [
						'hello',
						'world'
					]
					echo $one . '<br />';  // hello
					echo $two . '<br />';  // world
				```	 
			4. each($arr)循环: 接收一个数组作为参数，依次迭代该数组，先将数组的第一项返回，0/key为索引，1/value为值；直到最后无数组返回，则返回false 	
				```
					$arr = [
						'China',
						'USA'
					];
					$first = each($arr);
					echo '<pre>';
					var_dump($first);
					echo '</pre>';
					/* array(4){
					*		[1] => 
					*		string(5) China
					*		[value] =>
					*		string(5) China
					*		[0] => 
					*		int(0)
					*		['key'] =>
					*		int(0)
					*	}
					*/
				```
		* 数组常用操作函数
			array_shift：弹出数组中第一个数
			array_unshift：在数组头部压入元素
			array_pop：弹出数组中最后一个数
			array_push：在数组尾部压入元素
			current：数组当前指针的值
			key：数组当前指针的键
			next：指针向下移
			prev：指针向下移
			reset：重置指针到开始处
			end：指针到结束处		
### 正则表达式	
	界定符: 除了a-zA-Z0-9\()不能做界定符，其他都可以，但要成对出现
	原子：
		\d：匹配0-9的数字
		\D：匹配非0-9的字符
		\s：匹配空格
		\S：匹配非空格
		\w：匹配0-9或a-z或A-Z或_
		\W：匹配非0-9a-zA-Z_
		[]：匹配指定范围
		[^]：匹配非区间字符
	元字符：	
		+：匹配前面的字符至少一次
		*：匹配0次或任意次
		?：匹配0次或1次
		.：匹配除换行符之外的任意字符
		|：或者，优先级最低 $reg = '/abc|bcd/' 匹配abc或者bcd
		^：以某个字符开头
		$：以某个字符结尾
		\b：词边界
		\B：非词边界
		{m}：匹配的字符出现m次
		{n, m}：匹配的字符出现n到m次
		{n,}：匹配的字符至少出现n次
	模式修正符：
		i：表示不区分大小写
		m：字符串视为多行，如果模式中没有^或$或者字符串中没有换行，则此模式无效
		s：将字符串视为一行，换行符作为普通的字符
		x：忽略模式中的空白符
		U：非贪婪匹配
		A：从目标字符串的开头开始匹配
		D：如果设置此修正符则$会匹配到字符串结尾，如果未设置，结尾有换行符$会匹配此字符之前	
	方法
		1. preg_match($reg, $string, [$array] /* optinal */): 接收一个正则表达式参数、要测试的字符串、和可选的匹配到的数组，如果匹配则返回匹配到的个数，并将匹配到的值赋值给数组，否则返回0
		```
			$reg = '/aaa/';
			$str = 'fapfdaeaaa';
			if(preg_match($reg, $str, $arr)){
				print_r($arr); // Array([0] => aaa)
			}
		```	
### 文件操作	
	1. readfile('dirname'): 读文件
	2. file_get_contents('dirname'): 读文件，将内容赋值给一个变量
	3. fopen('filename', 'module')：打开一个文件，返回资源，一个文件路径参数，一个模式参数
		module：
			r：只读模式打开，将文件指针指向文件头
			r+：读写模式打开，将文件指针指向文件头
			w：写入模式打开，将文件指针指向文件头并将文件大小截为0，如果文件不存在则尝试创建
			w+：读写模式打开，将文件指针指向文件头并将文件大小截为0，如果文件不存在则尝试创建
			a：写入方式打开，将文件指针指向文件末尾，如果文件不存在则尝试创建
			a+：读写方式打开，将文件指针指向文件末尾，如果文件不存在则尝试创建
			x：创建并以写入方式打开，将文件指针指向文件头，如果文件存在则调用fopen失败，并返回false，并产生错误信息，如果文件不存在则尝试创建
			x+：创建并以读写方式打开，将文件指针指向文件头，如果文件存在则调用fopen失败并返回false，并产生错误信息，如果文件不存在则尝试创建
	4. fread('resource', size)：读取资源，传入一个资源，和读取长度，返回资源内容
	5. fclose('resource')：关闭资源 	
	6. file_put_contents('dirname', 'data')：写入数据，返回写入的数据长度
	7. fwrite(resource, data)：写入数据，传入一个资源和数据，返回写入的数据长度
	8. tmpfile()：创建临时文件，用完关闭即删。
		```
			$file = tmpfile();
			fwrite($file, 'data');
			fclose($file);
		```
	9. rename('old_filename', 'new_filename')：重命名文件名
	10. copy('old_file', 'new_file')：拷贝文件
	11. unlink('filename')：删除文件
	12. 文件属性检测
		* file_exists：文件是否存在
		* is_file：是否是文件
		* is_dir：是否是文件夹
		* is_writeable：文件是否可写
		* is_readable：文件是否可读
		* is_executable：文件是否可执行	
	



