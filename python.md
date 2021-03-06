### 进入/退出Python交互模式
	打开命令行窗口输入python进入Python交互模式，输入`exit()`退出Python交互模式或者按`ctrl+z`然后enter退出

	print(): 打印字符串，接收一个或一组字符串，一组字符串中间用逗号分隔，且在解析的时候逗号被解析为空格
	执行Python文件: 进入文件所在目录后执行: python xxx.py(文件名只能是英文、数字、下划线组合)	
	每一行语句的开始不能有空格或者tab
	注释：#开头
### 字符串
	* Python有4种类型的数——整数、长整数、浮点数、复数
		1. 整数：2
		2. 长整数：大一些的整数
		3. 浮点数：带有小数点的数
		4. 复数
	* 字符串——单引号、双引号、三引号
		1. 单引号字符串''	：引号中制表符空格和tab都原样保留
		2. 双引号字符串""：跟单引号一致
		3. 三引号字符串'''：可以自由的换行，也可以在其内部插入单引号和双引号
		4. 字符串是不可变的
	* 转义——\
		例如在单引号字符串中想使用单引号，此时需要对要使用的单引号进行转义
			```
			print('What\'s your name');
			```
		如在一行的末尾加上转义符(\)；则此时表示字符串在下一行继续，而不是开始一个新行(同样适用于三引号字符串)
	* 自然字符串——想要指示某些不需要如转义符那样的特别处理的字符串，那么需指定一个自然字符串
		自然字符串通过给字符串加上前缀r或R
		```
			r"this is a string \n" #此时\n不会被转义，即不是一个换行符，如果不加前缀r则是一个换行符
		```	
	* 字符串方法
		* str.lower()				# 将字符串转换为小写
		* startswith()——判断是否以某个字符串开头
		* in——判断字符串中是否有某个字符
		* find()——返回某个字符在字符串中的索引，没有则返回-1	
        * strip(string)——如string省略则默认删除字符串前后空格，包含回车符、制表符、换行符，如有参数则删除字符串前后含有string组成的字符，直到遇到不包含在string中的字符为止
### 变量
	1. 标识符——以英文(大写或小写)或_开头，后面可以跟英文或者数字或者下划线，标识符对大小写敏感
	2. 声明变量时只需赋值，不需要声明变量类型
	3. 在内部函数可以使用外部函数的变量，但是不能修改，修改的话会报错，除非使用global声明是全局变量才能修改
	4. 在内部函数要修改外部函数的变量要在内部函数通过nonlocal声明，否则会报错
### 数据类型
	数、字符串和类、Boolean(True, False)		
	1. 数据类型转换
		* int()——转换成整数
		* float()——转换成浮点数
		* str()——转换成字符串
		* bool()——转换成布尔值('', 0, 空对象, 空数组, 空set为False)
	2. 数据类型判断
		* 内置的isinstance(需要判断的变量, int/float/str/[int, float])	
### 运算符
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
### 控制语句
	1. if控制语句
		```
			if expression1: 
				statement1
				statement2				# optinal
			elif expression2:				# optinal
				statement1
				statement2				# optinal 
			else: 	#optinal
				statement		
		```	
	2. while语句——可选的else语句，当expression为False后执行
		```
			while expression: 
				statement1
			else: 				# optional
				statement1	
		```	
	3. for...in循环——迭代队列中的每个项目
		```
			for i in range(1, 5, [step] optional): 				# 不包含5
				print(i)
			else: 				# optinal，在for循环语句结束后执行，除非遇到break语句
				print('The loop end!')	
		```	
	***break——终止循环，此时while和for的else语句不会执行***
	***continue——跳出当前循环，继续下一轮循环，else语句执行***
### 函数
	1. 定义
		```
			def function_name(): 
				statement
		```	
	2. 作用域——定义在函数内部和外部的同名变量没有关系，内部函数只在内部有效	
	3. 若要在函数内部使用外部定义的变量，则在函数调用前该变量已经声明，否则报错
		```
			def fn(): 
				print(x)
			x = 10				# 这行变量的定义一定要在函数调用前，否则报错
			fn()	
		```
	4. 在函数内容使用global来使用定义在函数外部的变量，若在函数内部修改该变量，则全局变量也变
		```
			def fn(): 
				global x 				# 指定多个全局变量global x,y,z
				print(x)				# x == 5
				x = 10
				print(x) 				# x == 10
			x = 5
			fn() 	
		```	
	5. 默认值——可以在函数形参后赋一个默认值(只能将有默认值的形参放在函数形参列表的最后，默认参数要是不可变对象)
		```
			def fn(l = []): 
				l.append('hello')
			fn() 				# l = ['hello']
			fn() 				# l = ['hello', 'hello']	
		```
	6. 关键参数——在调用函数的时候将值直接传入，则不用担心传入参数的顺序
		```
			def fn(a, b = 10, c = 20): 
				print(a, b, c)
			fn(30)				# 30 10 20
			fn(b = 30, a = 40)				# 40 30 20
			fn(30, 40)				# 30 40 20	
		```	
	7. return语句——从函数中返回值
		如果不指定return语句，则默认返回None	
	8. 文档字符串__doc__是对象的一个属性，在对象中用多行字符串定义
		```
			def fn(x, y): 
				'''This is a docstring
				test test test.'''
			print(fn.__doc__)	
		```
	9. 空函数——可以定义一个空函数什么都不用做，但是要传入pass，pass是一个占位符
		```
			def fn():
				pass				# 此时不会报错，缺少pass则报错
		```	
	10. 可变参数——在函数形参前加上*，即可让参数变成形参，这可以让函数在调用时传入不定个数实参
		```
			def fn(*numbers): 
				sum = 0
				for number in numbers: 
					sum = sum + number ** 2
				return sum
			fn(1, 2, 3)
			fn(1, 2, 3, 4)
			list = [1, 2, 3, 4, 5]
			fn(*list)		
		```	
	11. 关键字参数——关键字参数允许传入任意个含参数名的参数，这些参数在函数内部自动组装成一个dict
		```
			def person(name, age, **key): 
				print('Name: ', name, 'Age: ', age, 'Others: ', key)
			person('Bill', 23, city = 'Beijing') # key = { 'city': 'beijing' }
			dict = { 'score': 60, 'city': 'Shanghai' }
			person('Gate', 24, **dict)	
		```	
	12. 匿名函数——lambda后接参数和冒号，然后跟上表达式(有且只有一个)，没有return语句
		```
			print(list(filter(lambda x: x % 2 == 0, range(0, 20))))
		```	
	13. 函数有一个__name__属性，返回函数的名字	
### 模块化
	1. 通过import module导入模块，则在模块中定义的变量和函数是该模块的属性
		```
			import module
			module.fn()
			module.property
		```
	2. 通过from module import fn, property导入模块，则可以直接调用fn和property，未导入的不能用
		```
			from module import fn, property
			fn()
			property
		```	
	3. 为了防止模块名冲突可以将模块放入一个文件夹中，导入时导入文件夹，该文件夹中必须存在__init__.py模块，读取该模块通过：dirname.modulename
	4. 模块名不要与系统模块名重复，检测系统中是否存在该模块可以通过在python交互环境下import模块，如果成功则存在，否则会报无该模块的错误	
	5. 在模块内部使用的函数或者变量，约定用法是在前加_下划线表示私有
	6. 导入和内建同名的模块可以使用from xxx import xxx
### 列表/数组
	1. 定义
		```
			list = [item1, item2, ...]
		```		
	2. 获取列表长度——len(listName)
	3. 列表排序——list.sort() #影响列表本身
	4. 追加列表项——list.append(item)
	5. 删除列表项——del list[index]或list.pop(index)
	6. 插入列表项——list.insert(index, item)
	7. 删除末尾列表项——list.pop
	8. 获取索引——list.index(item)
### 元组tuple——不可变，用圆括号包裹的一系列值
	```
		zoo = ('monkey', 'elephen', ...)
	```	
	1. 获取长度——len(zoo)
	2. 访问——zoo[index]
	如果定义只有一个项的元祖，则第一个项后要加逗号—— zoo = ('monkey', )
	```
		age = 23
		name = 'Bill'
		print('%s is %d years old'%(name, age))				# 'Bill is 23 years old' $s表示字符串，%d表示整数，%f表示浮点数，%x表示十六进制整数，%要是一个普通字符的话，则需要转义%%
		另一种格式化的方式：format
			print('Hello, {0}, your age is {1}'.format('Bill', 23))
		print('Why is %s playing with that python?'%name)				# 一个不需要加括号
	```
	3. 元祖的项可以使用变量一一对应赋值
		```
			m, e = ('monkey', 'elephen')
			print(m, e)				# m = 'monkey', e = 'elephen'
		```
### 字典/对象——键要是字符串
	1. 获取长度——len(obj)
	2. 获取项数——obj.items()
	3. 添加项数——obj[key] = value
	4. 删除项数——del obj[key]或obj.pop(key)
	5. 检验字典是否存在某个项数——in操作符或者obj.get(item)没有该item则返回None或者obj.get(item, -1)没有该item则返回-1
	6. 获取key——obj.keys()，返回一个list
	7. 获取value——obj.values()，返回一个list
	8. 迭代
		```
			dict = { 'a': 1, 'b': 2, 'c': 3}
			for i in dict: 				# 相当于: for i in dict.keys(): 
				print(i)				# a,b,c
			for i in dict.values(): 
				print(i)				# 1,2,3
			for k,v in dict.items(): 
				print(k, '=', v)				# k = a,b,c v = 1,2,3	
		```
### set——不重复元素的集合
	```
		s = set([1, 2, 3, 2, 1])
		print(s) #{1, 2, 3}
	```	
	1. 添加——s.add(item)				# 添加重复的没有效果
	2. 移除——s.remove(item)
	3. 两个set交集——s1 & s2
	4. 两个set并集——s1 | s2
### 序列——列表、元祖和字符串都是序列，索引操作符可以从序列中抓取一个特定项目，切片操作符可以获取序列的一系列项目
	```
		shoplist = ['apple', 'banana', 'tomato', 'orange']
		print(shoplist[1])				# banana
		print(shoplist[-2])				# tomato，索引超过范围会报错
		print(shoplist[: ]) #输出整个列表，如果第一个数不指定则从头开始，若第二个数不指定则到最后
		print(shoplist[1: 3])				# banana, tomato
		print(shoplist[0: -2])				# apple banana
	```	
	赋值操作符将一个对象赋值给另一个对象时，他们引用的是同一个对象的引用地址，当用切片操作符时，他们引用的是不同的对象
### 类
	1. 声明一个类
		```
			class Person(): 
				def sayHello(self, x, y = 4):				# 声明方法要传入self参数
					print('Hello, World!', x, y)
			p = Person()
			p.sayHello(x = 5)		
		```
	2. __init__方法——相当于构造函数
		```
			class Person(): 
				def __init__(self, name, age): 
					self.name = name
					self.age = age
				def sayName(self): 
					print('My name is', self.name)
				def sayAge(self): 
					print('My age is', self.age)
			p = Person('Bill', 23)
			p.sayName()
			p.sayAge()				
		```	
	3. 继承
		```
			class SchoolMember: 
				def __init__(self, name, age): 
					self.name = name
					self.age = age
					print('Initialize schoolmember %s'%self.name)
				def tell(self): 
					print('Name: %s Age %s'%(self.name, self.age))
			class Teacher(SchoolMember): 
				def __init__(self, name, age, salary): 
					SchoolMember.__init__(self, name, age)
					print('Initialize teacher %s'%self.name)
				def tell(self): 
					SchoolMember.tell(self)
					print('My salary is %d'%self.salary)
			class Student(SchoolMember): 
				def __init__(self, name, age, score): 
					SchoolMember.__init__(self, name, age)
					print('Initialize student %s'%name)
				def tell(self): 
					SchoolMember.tell(self)
					print('My score is %d'%self.score)
			t = Teacher('Bill', 28, 4500)
			s = Student('Gate', 14, 96)
			members = [t, s]
			for member in members: 
				member.tell()									
		```	
### [内置函数](https://docs.python.org/3/library/functions.html)
	1. range(start, end, [step])——依次返回范围内的数，通常用于for循环
		```
			for i in range(1, 10, 2): 
				print(i)				# print: 1, 3, 5, 7, 9(不包括end)
		```
		* start不指定默认是0，step不指定默认是1，且step要不等于0
		* step可以接收负值
		* 项数计算：r[i] = start + step * i，当step为正时i >= 0 and r[i] < end，当step为负时i >= 0 and r[i] > end
### 迭代
	1. 迭代list的索引和值
		```
			for key, val in enumerate([1, 2, 3]): 
				print(key, val)				# 0 1, 1 2, 2 3
		```
	2. for in迭代dict迭代的是key相当于for key in dict.keys()
	3. for in迭代list和tuple迭代的是值	
### 列表生成式
	```
		list(range(1, 10))				# [1, 2, 3, 4, 5, 6, 7, 8, 9]
		[x ** 2 for x in range(1, 5)]				# [1, 4, 9, 16]
		[x ** 2 for x in range(1, 5) if x % 2 == 0]				# [4, 16]
		[x + y for x in range(1, 3) for y in range(3, 5)]				# [4, 5, 5, 6]
		L = ['Hello', 'World', 'IBM', 'Apple']
		[l.lower() for l in L]
	```		
### 生成器——generator(列表元素按照某种方法推算出来，让其在循环过程中不断计算，而不需要立即创建完整的list，从而节省空间)
	1. 创建
		```
			g = (i for i in range(10))
			next(g)		# 调用，计算到最后一个元素时，没有更多的元素会抛出StopIteration错误
			for i in g: 
				print(i)				# 通过循环调用
		```	
		* 斐波那契
			```
				def fib(max): 
					a, b, n = 0, 1, 0
					while n < max: 
						print(b)
						a, b = b, a + b
						n += 1	
			```
		* 第二种创建——yield(如果函数中含有yield，则表明这个函数不是普通的函数，而是一个yield)
			```
				def fib(max): 
					a, b, n = 0, 1, 0
					while n < max: 
						yield b
						a, b = b, a + b
						n += 1
					return 'Done'				# generator用for循环调用时是无法取得函数的返回值的，因为每一次调用是截止到yield，而下次调用会继续执行上一次yield之后的语句，而用next()可以捕获到，当抛出StopInteration错误后可以捕获，返回值包含在StopIteration的value中		
			```
			yield和普通函数执行流的区别，普通函数在执行的过程中，当遇到return语句后会返回，而generator会在遇到yield语句返回，下次执行的时候，会继续执行上一次yield后的语句
			```
				g = fib(6)
				while True: 
					try: 
						print(next(g))
					except StopIteration as e: 
						print(e.value)
						break	
			```
### 迭代对象——可以使用for循环迭代的对象，如：list/tuple/dict/set/str  (Iterable)
	```
		# 判断迭代对象
		from collections import Iterable
		bool = isinstance({}, Iterable)
	```
	可以被next()函数调用并不断返回下一个值的对象成为迭代器(Iterator)
	Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用不断返回下一个数据，直到没有数据返回StopIteration错误，是无法提前知道序列的长度
	如：str/dict/list/tuple都不是Iterator，可以使用iter()函数将其转换成Iterator
	```
		from collections import Iterator
		bool = isinstance((x for x in range(10)), Iterator) 				#这里的数据类型不是tuple而是generator，且返回true
		tuple = (1, 2, 3, 5)
		bool = isinstance(tuple, Iterator)				# 此时返回false
		bool = isinstance(iter(tuple), Iterator)
	```
### 三个高阶函数——map/reduce/sorted
	1. map——参数：一个作用于每一项的函数和一个Iterable对象，返回新的Iterator对象，可以用list()将其转换为list
		```
			def multi(x): 
				return x ** 2
			l = [1, 2, 3, 4]
			print(list(map(multi, l)))
		```
	2. reduce——参数：把函数作用域一个序列上，接收两个参数，把结果作为下一次的初始值
		```
			from functools import reduce
			def sum(x, y): 
				return x + y
			l = [1, 2, 3, 4]
			print(reduce(sum, l))	
		```
	3. sorted——参数：一个list和可选的接收排序函数的key，以及倒序排序的reverse = True，如果接收的是一个tuple，则返回一个list，原tuple不变，因为tuple是不变的
		```
			L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
			def sortByScore(tuple): 
				return tuple[1]
			print(sorted(L, key = sortByScore))	
		```	
	4. filter——过滤一个序列，参数：过滤的函数(保留返回True的项数)和一个要过滤的序列	，返回Iterator
		```
			# 求素数
			def prime(arr):
				max_num = max(arr)
				def removeTwo(x): 
					return x % 2 != 0 or x == 2
				arr = list(filter(removeTwo, arr))	
				n = 1
				while n < max_num: 
					n += 2
					def removeOther(x): 
						return x % n != 0 or x == n
					arr = list(filter(removeOther, arr))	
				return arr		
		```
### 面向对象
	1. 获取对象的信息		
		* 判断基本数据类型：type()函数/isinstance()函数
			```
				type(12) => init
				type(1.2) => float
				type(True) => bool
				type('string') => str
				isinstance(12, int)
			```			
		* 判断一个对象是否是函数：用type(variable)和types模块中的常量来判断
			```
				import types
				def fn(): 
					pass
				type(fn) == types.FunctionType
				type(abs) == types.BuiltinType
				type((x for x in range(10))) == types.GeneratorType
				type(lambda x : x ** 2) == types.LambdaType	
			```	
		* 判断类的继承关系：isinstance(instance, Class)	
		* 获取一个对象的所有的属性：dir(dict)返回一个包含所有属性的list
		* 判断对象是否有某个属性/方法：hasattr(dict, property)
		* 设置对象的某个属性：setattr(dict, property_name, property_value)
		* 获取对象的某个属性值：getattr(dict, property_name, [error_status])，如果获取属性不存在，则报错，可以设置一个可选的报错代码
	2. 类属性——在类中定义的属性，类本身和实例都可以访问，若是实例中的属性和类属性同名则获取时返回的是实例属性
		```
			class Student(object): 
				grade = 'One'
				def __init__(self, name, age): 
					self.name = name
					self.age = age
			s1 = Student('Bill', 23)
			s1.grade == Student.grade		
		```	
	3. 动态特性——Python是一门动态语言，可以在程序动态的添加属性和方法，在实例中动态添加的属性和方法只属于该实例，在类中动态的添加属性和方法属于通过该类创建的实例
		```
			class Student(object): 
				def __init__(self, name): 
					self.name = name
			s1 = Student('Bill')
			s1.age = 23				# age的属性只属于s1
			def sayName(self): 
				return self.name
			from types import MethodType
			s1.sayName = MethodType(sayName, s1)				# sayName的方法只属于s1
			Student.sayName = sayName  		# sayName的方法现在属于通过Student创建的实例			
		```	
	4. 限制实例的属性——可以通过在父类中添加__slots__ = (property1, property2, ...)来限制在实例中创建的属性
		```
			class Student(object): 
				def __init__(self, grade):				# 报错，因为__slots__属性中没有grade
					self.grade = grade
				__slots__ = ('name', 'age')				# 表明通过Student创建的实例只能添加name和age属性，添加其他属性报错，但在Student类和其子类中添加的属性不影响，也可以在Student的子类中添加__slots__属性来限制子类的实例添加的属性
			class Person(Student): 
				__slots__ = ('grade')
			p1 = Person()				# p1可以添加的属性有：name, age, grade		
		```	
	5. @property设置和获取属性——允许只定义getter	
		```
			import time
			class Student(object): 
				def __init__(self, name, birth):				#创建Student实例的时候需传入name和birth初始化属性
					self.__name = name
					self.__birth = birth
				@property
				def getName(self):				# getName是属性而不是方法
					return self.__name
				@getName.setter
				def getName(self, newName): 
					if not isinstance(newName, str): 
						raise ValueError('New name must be a string')
					self.__name = newName
				@property
				def getBirth(self): 
					return self.__birth
				@getBirth.setter
				def getBirth(self, newBirth): 
					if not isinstance(newBirth, int): 
						raise ValueError('New birth must be a int')
					self.__birth = newBirth
				@property
				def age(self):				# 只获取age属性
					return time.gmtime().tm_year - self.__birth
		```
	6. [多重继承](https://kevinguo.me/2018/01/19/python-topological-sorting/)——一个类继承多个父类的属性和方法
    ```
        class Mammal(object): 
            def drinking(self): 
                print('I\'m is drinking!')
        class RunningMixin(object): 
            def running(self): 
                print('I\'m is running!')
        class Dog(Mamal, RunningMixin):    # 继承Mamal和RunningMixin类     
            def __init__(self, name):              
                self.name = name
        d = Dog('Eddi')
                       
    ```    
### IO操作
	1. 操作文件
		* 读文件——open(dirname, module)
			```
				f = open('./test.txt', 'r')				# r表示只读模式,如果文件不存在则报错
				f.read()				# 表示读取文件内容
				f.close()				# 关闭文件
			```    
			由于文件读写时可能会产生错误，一旦出错则后续的close方法无法调用,可以使用try语句或者Python内置的with语句
			```
				try: 
					f = open('./test.txt', 'r')
					print(f.read())
				finally: 
					if f: 
						f.close()	
				------------------------
				with open('./text.txt', 'r') as f:				# 会自动调用close()方法 
					print(f.read())		
			```
			调用read()会一次性读取所有的数据，可以调用read(size)size表示字节来读取一部分数据，调用readline()读取一行数据，调用readlines()一次读取所有的内容并按行返回一个list
			二进制文件
				```
					with open('./test.jpg', 'rw') as f: 
						print(f.read())
					f.close()	
				```
			如果读取非utf-8类型的文件，open接收第三个参数encoding，如果文件编码不规范会抛出UnicodeDecodeError，可以使用第四个参数忽略错误
				`with open('./test.txt', 'r', ecoding = 'gbk', errors = 'ignore')`	
		* 写文件
			```
				f = open('../test/txt', 'w')             # 如果文件不存在则创建
				f.write(content)
				f.close()
			```
			可以反复调用write()来写文件，但是务必调用close()来关闭文件，当操作写文件时，操作系统一般会将内容存储在内存中，空闲的时候再写入，只有调用close()方法才能保证把所有的内容都写入到磁盘，如果w模式下文件已经存在，则文件会直接覆盖，若想追加module改为'a'
    2. 操作内存数据
        ***steam position***
        * 字符串
            ```
                from io import StringIO
                f = StringIO('string') # 未初始化数据，接收一个可选的初始化数据
                f.write('xxxx') # 写入数据
                f.getvalue()                # 获取数据
            ```  
            --------------------------------------------- 
            ```
                # 初始化数据后可以通过读文件操作来读取数据
                from io import StringIO
                s = StringIO('This is first string \n This is second string \n This is third string')
                for i in s.readlines(): 
                    print(i.strip())
            ```
        * 二进制
            ```
                from io import BytesIO
                f = BytesIO()
                f.write('编程'.encode('utf-8'))
                print(f.getvalue())
            ```
            ---------------------------------------------
            ```
                # 初始化二进制后可以通过读文件来读取数据
                from io import BytesIO
                b = BytesIO('\xe7\xbc\x96\xe7\xe8\x8b')
                print(b.read())
            ```
    3. 操作文件和目录
        ```
            # 文件模块由os模块提供
            import os
            print(os.path.abspath(''))              # 返回当前目录的绝对路径
        ```
        os.path.join(dirname1, dirname2)            # 组合两个路径
        os.path.split(dirname)
        os.path.splitext(dirname)
        os.mkdir(dirname)
        os.rmdir(dirname)
        os.rename('ori_filename', 'new_filename')
        os.remove(filename)
        ```
            # 复制文件
            with open('./test.txt', 'r') as ori_file: 
                with open('./testcopy.txt', 'w') as copy_file: 
                    copy_file.write(ori_file.read())
        ```
        ----------------------------------------------------
        ```
            import os
            print([x for x in os.listdir('.')])      # 返回当前目录下所有的文件/文件夹
            print([x for x in os.listdir('.') if os.path.isdir(x)])             # 返回当前目录下所有的文件夹
            print([x for x in os.listdir('.') if os.path.isfile(x)])    # 列出当前目录下所有的文件
            print([x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[0] == '.py'])    # 列出所有以.py结尾的文件
            os.path.relpath(path, start = os.curdir)
        ```


