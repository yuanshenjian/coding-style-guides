
* content
{:toc}

## 源代码布局
* 所有源文件以UTF-8编码
* 使用2个空格的缩进

	```ruby
	# 差 - 四个空格
	def some_method
	    do_something
	end
	
	# 好
	def some_method
	  do_something
	end
	```
	* 一般情况下专业的`IDE`中`tab`就可以满足，但不要迷信基于操作系统的`tab`
	
* 使用Unix风格的换行符`\n`
	* 如果你使用 Git，可用下面这个配置来保护你的项目不被 Windows 的换行符干扰
	
		```
			可使用git config --global core.autocrlf true 防止产生windows风格的换行符
		```
		
* 一行只写一条语句，不要使用`;`
	
	```ruby
	# 差
	puts 'foo'; puts 'bar' # 一行里有两个表达式
	
	# 好
	puts 'foo'
	puts 'bar'
	```
	
* 不要单行写方法，实在要有，不要超过一个表达式
	
	```ruby
	# 不忍直视
	def some_method; do_something; do_something_else; end
	
	# 差
	def some_method; do_something; end

	# 好
	def some_method
		do_something
	end
	```
	
* 操作符前后适当增加空格
	* 在 `,` `:` `;`后增加空格，在`+` `-` `*` `/` `=` 前后都增加空格，可增加代码的可读性
		
		```ruby
		sum = 1 + 2
		a, b = 1, 2
		```
		
* `[` `]` `{` `}` `(` `)` 以及 `**`不要加空格
		
	```ruby
	some_method(arg1, arg2)
	[1, 2, 3].size
	{one: 1, two: 2}
	```
	
* `!` 之后不要加空格	
	
	```ruby
	# 差
	! something
	
	# 好
	!something
	```
	
* 范围的字面量语法中，不要添加任何空格
	
	```ruby
	# 差
	'a' ... 'z'
	
	# 好
	'a'...'z'
	```
	
* `case`和`when`处于同一代码层次

	```ruby
	# 差
	case
	  when song.name == 'Misty'
	    puts 'Not again!'
	  else
	    song.play
	end
	
	# 好
	case
	when song.name == 'Misty'
	  puts 'Not again!'
	else
	  song.play
	end
	```
	
* 方法之间插入空行，方法内部使用空行分隔相关逻辑

	```ruby
	def init
	  data = initialize(options)
	
	  data.manipulate!
	
	  data.result
	end
	
	def eat
	  eat_something
	end
	```

* 对于一个方法有多个参数导致太长的时候，按如下方式处理

	```ruby
	def send_mail(source)
	   Mailer.deliver(to: 'bob@example.com',
	                  from: 'us@example.com',
	                  subject: 'Important message',
	                  body: source.text)
	end
	```
	
* 避免在方法调用的最后一个参数之后添加逗号

	```ruby
	# 差
	some_method(size, count, color, )
	
	# 好
	some_method(size, count, color)
	```

* 避免使用续行符 `\`，除了长字符串

	```ruby
	# 不忍直视
	result = 1 - \
	         2
	
	# 差
	result = 1 \
	         - 2
	
	# 可以接受
	long_string = 'First part of the long string' \
	              ' and second part of the long string'
	```

* 使用统一的风格进行多行链式方法调用
	* (风格 A)当多行链式方法调用需要另起一行继续时，将 . 放在第二行开头
		
		```ruby
		one.two.three
		  .four
		```
	* (风格 B)将 . 放在第一行末尾，以表示当前表达式尚未结束。 
	
		```ruby
		one.two.three.
		  four
		```

* 构建数组时，若元素跨行，应当保持对齐

	```ruby
	# 差 - 没有对齐
	menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
	  'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
	  
	# 好
    menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
                 'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
    # 非常好
    menu_item = [
        'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
        'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam'
    ]
	```

* 使用 _ 语法改善大数的数值字面量的可读性

	```ruby
	# 差
	num = 1000000
	
	# 好
	num = 1_000_000
	```

* 不要使用区块注释

	```ruby
	# 差
	=begin
	comment line
	another comment line
	=end
	
	# 好
	# comment line
	# another comment line
	``` 

* 使用Rdoc生产系统的API文档，在注释和def之间不要有空行

* 每行不超过80行

* 每行的结尾不要有空白字符

>源代码的排版，一般专业的IDE都提供了格式化快捷键进行格式化，如`RubyMine`中`[option]+[command]+L`

## 语法
1. 一个方法如果没参数就省略括号，有参数就使用括号
2. 如无必要，不要使用`for`，而使用`each`做循环
3. 不要使用`then`
4. 使用三元操作`? : `代替`if...else`
5. 不要在`if...else`的内部使用`? : `操作符
6. 使用`when X then ...`，因为它在ruby1.9被删除了
7. 使用`&&`和`||`做boolean操作，使用`and`和`or`做流程控制
8. `unless`和`else`不要一起用
9. 除非是判断条件涉及赋值操作，否则不不需要括起来
10. 把多行语句块使用`{}`包含，
11. 不需要使用的`return`的时候就不要用
12. 不要使用`\`连接2行代码
13. 使用`||=`来初始化变量，但是不能用来初始化`boolean`变量
14. 运行Ruby的时候，加上`-w`以提示我们代码中不好的地方
15. 使用`Ruby1.9`的语法写lambda和hash

--

## 命名规范
1. 使用小写+`_`命名变量和方法
2. 使用首字母大写命名Module和Class
3. 使用全大写+`_`命名常量
4. 对于返回值是boolean的方法加个`?`后缀
5. 对于一些有潜在风险的方法加`!`后缀，比方说有exit，修改了self，或者变量等等

--

## 注释
1.代码即注释，努力消除注释

--

## 类
1. 符合liskov原则，子类可以替换父类
2. 尽量让类做到SOLID (Single responsibility, Open-closed, Liskov substitution, Interface segregation and Dependency inversion)
3. 为每个类都写一个`to_s`的方法以查看类的状态
4. 使用attr家族的方法做类属性的访问控制
5. 考虑增加新的工厂方法做一些有意义的实例初始化工作
6. 使用DuckTyping而非继承因为动态语言的特性，不在需要多态了
7. 避免使用`@@`，全局变量，
8. 根据访问情况，合理使用访问控制符
9. 使用self来定义单例方法，而不是使用类名

--

## 异常
1. 不要放过一些异常
2. 不要使用异常做流程控制
3. 不要捕获Exception，异常基类
4. 根据异常类型的覆盖面排列异常
5. 把所有的外部资源放到异常捕获模块中
6. 优先使用库自带的异常，而不是自己创建异常

--

## 集合
1. 优先使用`%w`创建字符串数组
2. 按需创建数组
3. 使用Set去除List中的重复元素
4. 使用Symbol做Hash key，而不是String，不要使用可变对象做Hash Key
5. 不要在遍历一个列表的同时，又在改变它

--

## Strings
1. 使用`#{String} #{string}`优于`String+String`
2. 未使用#{}形式的String时，使用`''`表示
3. 在做实例变量的连接时，不要使用{}
4. 使用<<而不是+做字符串串联

--

## 正则表达式
1. 使用命名组而非$1-9以便于跟踪
2. `^` `$`表示匹配整行，匹配整个字符串应使用`\A` `\Z`
3. 使用`x`修饰符修饰复杂的regex语句，增加可读性，但是注意空格的去除问题

--

## %的语法
1. 多使用`%w`
2. 需要字符串内嵌表达式的时候使用`%()`
3. 使用`%r`当正则表达式中出现多个`/`
4. 不要使用 `%q` `%Q` `%x` `%W` `%s`这些字符
5. 在`%`后优先使用`()`作为分隔符


## 参考
[Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide)

