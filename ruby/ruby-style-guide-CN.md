
* content
{:toc}




## 命名规范
>程序设计的真正难题是替事物命名及使缓存失效。
——Phil Karlton

* 标识符使用英文命名

	```ruby
	# 差 - 标识符使用非 ASCII 字符
	заплата = 1_000
	
	# 差 - 标识符使用拉丁文写法的保加利亚单词
	zaplata = 1_000
	
	# 好
	salary = 1_000
	```

* 符号、方法、变量使用蛇底式小写`snake_case`

	```ruby
	# 差
	:'some symbol'
	:SomeSymbol
	:someSymbol
	
	someVar = 5
	
	def someMethod
	  ...
	end
	
	def SomeMethod
	...
	end
	
	# 好
	:some_symbol
	
	def some_method
	  ...
	end
	```

* 类与模块使用驼峰式大小写`CamelCase`。(`HTTP`、`XML`等首字母缩写应该仍旧保持大写形式)

	```ruby
	# 差
	class Someclass
	end
	
	class Some_Class
	end
	
	class SomeXml
	end
	
	class XmlSomething
	end
	
	# 好
	class SomeClass
	end
	
	class SomeXML
	end
	
	class XMLSomething
	end
	```

* 文件名使用蛇底式小写，如`hello_world.rb`

* 谓词方法（返回布尔值的方法）的名字应当以问号结尾，比如`Array#empty?`。返回值不是布尔值的方法不应以问号结尾

* 具有潜在危险性的方法，当其存在对应安全版本的方法时，其名字应当以惊叹号结尾

	```ruby
	# 差 - 没有对应安全版本的方法
	class Person
	  def update!
	  end
	end
	
	# 好
	class Person
	  def update
	  end
	end
	
	# 好
	class Person
	  def update!
	  end
	
	  def update
	  end
	end
	```

## 源代码布局

* 所有源文件以UTF-8编码	
* 使用Unix风格的换行符`\n`
	* 如果你使用 Git，可用下面这个配置来保护你的项目不被 Windows 的换行符干扰
	
		```
		$ git config --global core.autocrlf true 防止产生windows风格的换行符
		```

### 把控好嵌套块的缩进

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

### 单行VS多行
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

### 灵活使用空格和空行

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

* 使用Rdoc生产系统的API文档，在注释和def之间不要有空行

* 每行的结尾不要有空白字符

### 享受对齐的美感
* 对于一个方法有多个参数导致太长的时候，按如下方式处理

	```ruby
	def send_mail(source)
	   Mailer.deliver(to: 'bob@example.com',
	                  from: 'us@example.com',
	                  subject: 'Important message',
	                  body: source.text)
	end
	```

* 避免使用续行符`\`，除了长字符串

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

* 每行不超过80行


>源代码的排版，一般专业的IDE都提供了格式化快捷键进行格式化，如`RubyMine`中`[option]+[command]+L`

## 语法

### 清晰的赋值

* 使用`_`语法改善大数的数值字面量的可读性

	```ruby
	# 差
	num = 1000000
	
	# 好
	num = 1_000_000
	```

* 定义变量时，避免并行赋值。但当右值为方法调用返回值，或是与`*`操作符配合使用，或是交换两个变量的值，并行赋值也是可以接受的。并行赋值的可读性通常不如分开赋值。

	```ruby
	# 差
	a, b, c, d = 'foo', 'bar', 'baz', 'foobar'
	
	# 好
	a = 'foo'
	b = 'bar'
	c = 'baz'
	d = 'foobar'
	
	# 好 - 交换两个变量的值
	a = 'foo'
	b = 'bar'
	
	a, b = b, a
	puts a # => 'bar'
	puts b # => 'foo'
	
	# 好 - 右值为方法调用返回值
	def multi_return
	  [1, 2]
	end
	first, second = multi_return
	
	# 好 - 与 * 操作符配合使用
	first, *list = [1, 2, 3, 4]
	
	hello_array = *'Hello'
	a = *(1..3)
	```

* 除非必要，否则避免在并行赋值时使用单字符的`_`变量

	```ruby
	foo = 'one,two,three,four,five'
	
	# 差 - 可有可无，且无任何有用信息
	first, second, _ = foo.split(',')
	first, *_ = foo.split(',')
	
	# 好
	a, = foo.split(',')
	a, b, = foo.split(',')
	
	# 可有可无，但提供了额外信息
	first, *_ending = foo.split(',')
	
	# 好 - 占位符，_ 担当最后一个元素
	*beginning, _ = foo.split(',')
	```	

* 优先考虑简短的自我赋值语法

	```ruby
	# 差
	x = x + y
	x = x * y
	x = x**y
	x = x / y
	x = x || y
	x = x && y
	
	# 好
	x += y
	x *= y
	x **= y
	x /= y
	x ||= y
	x &&= y
	```

* 当变量尚未初始化时，使用`||=`对其进行初始化

	```ruby
	# 差
	name = name ? name : 'Bozhidar'
	
	# 差
	name = 'Bozhidar' unless name
	
	# 好 - 当且仅当 name 为 nil 或 false 时，设置 name 的值为 'Bozhidar'
	name ||= 'Bozhidar'
	```
* 不要使用`||=`对布尔变量进行初始化

	```ruby
	# 差 - 设置 enabled 的值为 true，即使其原本的值是 false
	enabled ||= true
	
	# 好
	enabled = true if enabled.nil?
	
	# 好
	enabled = enabled.nil?
	```

* 使用`&&=`预先检查变量是否存在，如果存在，则做相应动作。使用`&&=`语法可以省去`if`检查

	```ruby
	# 差
	if something
	  something = something.downcase
	end
	
	# 差
	something = something ? something.downcase : nil
	
	# 勉强可以
	something = something.downcase if something
	
	# 好
	something = something && something.downcase
	
	# 更好
	something &&= something.downcase
	```


### 干脆利落的方法定义

* 定义方法时如果没参数省略括号，有参数时使用括号
	
	```ruby
	# 差
	def some_method()
	end
	
	# 好
	def some_method
	end
	
	# 差
	def some_method param1, param2
	end
	
	# 好
	def some_method(param1, param2)
	end
	``` 

* 定义可选参数时，将可选参数放置在参数列表尾部。否则可能产生预料之外的结果
	
	```ruby
	# 差
	def some_method(a = 1, b = 2, c, d)
	  puts "#{a}, #{b}, #{c}, #{d}"
	end
	some_method('w', 'x') # => '1, 2, w, x'
	some_method('w', 'x', 'y') # => 'w, 2, x, y'
	some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'
	
	# 好
	def some_method(c, d, a = 1, b = 2)
	  puts "#{a}, #{b}, #{c}, #{d}"
	end
	some_method('w', 'x') # => '1, 2, w, x'
	some_method('w', 'x', 'y') # => 'y, 2, w, x'
	some_method('w', 'x', 'y', 'z') # => 'y, z, w, x'
	```

* 不要在方法中嵌套定义方法，使用 lambda 方法来替代。 嵌套定义产生的方法，事实上和外围方法处于同一作用域（比如类作用域）。此外，“嵌套方法”会在定义它的外围方法每次调用时被重新定义

	```ruby
	# 差
	def foo(x)
	  def bar(y)
	    # 省略主体
	  end
	
	  bar(x)
	end
	
	# 好 - 作用同前，但 bar 不会在 foo 每次调用时被重新定义
	def bar(y)
	  # 省略主体
	end
	
	def foo(x)
	  bar(x)
	end
	
	# 好
	def foo(x)
	  bar = ->(y) { ... }
	  bar.call(x)
	end
	```

* 避免在不需要流程控制的情况下使用`return`

	```ruby
	# 差
	def some_method(some_arr)
	  return some_arr.size
	end
	
	# 好
	def some_method(some_arr)
	  some_arr.size
	end
	```


### 聊一聊lambda
* 对于单行区块，使用新的`lambda`字面量定义语法。对于多行区块，使用`lambda`定义语法

	```ruby
	# 差
	l = lambda { |a, b| a + b }
	l.call(1, 2)
	
	# 好 - 但看起来怪怪的
	l = ->(a, b) do
	  tmp = a * 7
	  tmp * b / 50
	end
	
	# 好
	l = ->(a, b) { a + b }
	l.call(1, 2)
	
	l = lambda do |a, b|
	  tmp = a * 7
	  tmp * b / 50
	end
	```

* 定义 lambda 方法时，如果有参数则使用括号
	
	```ruby
	# 差
	l = ->x, y { something(x, y) }
	
	# 好
	l = ->(x, y) { something(x, y) }
	```

* 定义 lambda 方法时，如果无参数则省略括号

	```ruby
	# 差
	l = ->() { something }
	
	# 好
	l = -> { something }
	```


### 循环的那些事
* 不要使用`for`， 除非你很清楚为什么，大部分情况下，你应该使用迭代器，使用`each`做循环
	* 因为`for`是由`each`实现的。另外，`for`没有引入一个新的作用域 (`each`有），因此在它内部定义的变量在外部仍是可见的

	```ruby
	arr = [1, 2, 3]
	
	# 差
	for elem in arr do
	  puts elem
	end

	# 注意，elem 可在 for 循环外部被访问
	elem # => 3
	
	# 好
	arr.each { |elem| puts elem }
	
	# 注意，elem 不可在 each 块外部被访问
	elem # => NameError: undefined local variable or method `elem'
	```	

* 在多行`while/until`中，不要使用`while/until ... do`

	```ruby
	# 差
	while x > 5 do
	end
	
	# 好
	while x > 5
	end
	```

* 对于无限循环，使用`loop do`,而不是`while/until`

	```ruby
	# 差
	while true
	  do_something
	end
	
	until false
	  do_something
	end
	
	# 好
	loop do
	  do_something
	end
	```

### 孰是孰非
* 在 case 表达式中，单行情况使用 `when x then ...` 语法。另一种语法 `when x: ...` 在 Ruby 1.9 之后被移除了

* 使用`!`而不是`not`

	```ruby
	# 差 - 因为操作符的优先级，这里必须使用括号
	x = (not something)
	
	# 好
	x = !something
	```

* 避免使用`!!`

	```ruby
	# 差
	x = 'test'
	# 令人费解的 nil 检查
	if !!x
	  # 省略主体
	end
	
	x = false
	# 对于布尔变量，双重否定是多余的
	!!x # => false
	
	# 好
	x = 'test'
	unless x.nil?
	  # 省略主体
	end
	```

* 使用`and`与`or`作为条件判断。使用`&&`与`||`来赋值和流程控制，通常只使用`&&`与`||`就好
	* 因为`and`和`or`的优先级问题，在赋值的时候会产生意料之外的结果
	
	```ruby
	# 好
	if some_condition and some_other_condition
	  do_something
	end
	
	# 差
	a = false or true # a = false

	
	差
	# 流程控制
	document.saved? or document.save!
	
	# 好
	a = false || true # a = true
	# 流程控制
	document.saved? || document.save!
	
	```

* 倾向使用谓词方法而不是`==`操作符。但数值比较除外

	```ruby
	# 差
	if x % 2 == 0
	end
	
	if x % 2 == 1
	end
	
	if x == nil
	end
	
	# 好
	if x.even?
	end
	
	if x.odd?
	end
	
	if x.nil?
	end
	
	if x.zero?
	end
	
	if x == 0
	end
	```

* 不做显式的 non-nil 检查，除非检查对象是布尔变量

	```ruby
	# 差
	do_something if !something.nil?
	do_something if something != nil
	
	# 好
	do_something if something
	
	# 好 - 检查对象是布尔变量
	def value_set?
	  !@some_boolean.nil?
	end
	```


### 玩转条件语句

* 不要在多行`if/unless`中使用 then

	```ruby
	# 差
	if some_condition then
	  # 省略主体
	end
	
	# 好
	if some_condition
	  # 省略主体
	end
	```

* 倾向使用三元操作符`? :`而不是`if/then/else/end`结构
	
	```ruby
	# 差
	result = if some_condition then something else something_else end
	
	# 好
	result = some_condition ? something : something_else
	```

* 当三元操作符的分支超过一个表达式（多行）时，使用`if...else`来降低复杂度

	```ruby
	# 差
	some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else
	
	# 好
	if some_condition
	  nested_condition ? nested_something : nested_something_else
	else
	  something_else
	end
	```

* 对于后置条件循环语句，倾向使用`loop do`与`break`的组合，而不是`begin...end until/while`
	
	```ruby
	# 差
	begin
	  puts val
	  val += 1
	end while val < 0
	
	# 好
	loop do
	  puts val
	  val += 1
	  break unless val < 0
	end
	```

* 对于单行主体，倾向使用`if/unless`修饰语法。另一种方法是使用流程控制`&&`或`||`

	```ruby
	# 差
	if some_condition
	  do_something
	end
	
	# 好
	do_something if some_condition
	
	# 好 - 使用流程控制
	some_condition && do_something
	```

* 避免使用嵌套`if/unless/while/until`修饰语法。适当情况下，使用`&&`或`||` 来替代
	
	```ruby
	# 差
	do_something if other_condition if some_condition
	
	# 好
	do_something if some_condition && other_condition
	```

* 对于否定条件，倾向使用`unless`而不是`if`（或是使用流程控制`||`）

	```ruby
	# 差
	do_something if !some_condition
	
	# 差
	do_something if not some_condition
	
	# 好
	do_something unless some_condition
	
	# 好
	some_condition || do_something
	```

* 避免在多行区块后使用`if/unless`修饰语法

	```ruby
	# 差
	10.times do
	  # 省略多行主体
	end if some_condition
	
	# 好
	if some_condition
	  10.times do
	    # 省略多行主体
	  end
	end
	```

* 不要使用`unless`与`else`的组合。将它们改写成肯定条件

	```ruby
	# 差
	unless success?
	  puts 'failure'
	else
	  puts 'success'
	end
	
	# 好
	if success?
	  puts 'success'
	else
	  puts 'failure'
	end
	```
	
* 流程控制中，避免使用嵌套条件
	* 倾向使用防御从句进行非法数据断言。防御从句是指处于方法顶部的条件语句，其能尽早地退出方法(短路)
	
		```ruby
		# 差
		def compute_thing(thing)
		  if thing[:foo]
		    update_with_bar(thing)
		    if thing[:foo][:bar]
		      partial_compute(thing)
		    else
		      re_compute(thing)
		    end
		  end
		end
		
		# 好
		def compute_thing(thing)
		  return unless thing[:foo]
		  update_with_bar(thing[:foo])
		  return re_compute(thing) unless thing[:foo][:bar]
		  partial_compute(thing)
		end
		```
	
	* 循环中，倾向使用`next`而不是条件区块
	

		```ruby
		# 差
		[0, 1, 2, 3].each do |item|
		  if item > 1
		    puts item
		  end
		end
		
		# 好
		[0, 1, 2, 3].each do |item|
		  next unless item > 1
		  puts item
		end
		```

###  洒脱的方法调用
* 对于 DSL 的内部方法（比如 Rake、Rails、RSpec）、具有“关键字”特性的内部方法（比如 attr_reader、puts）、以及属性存取方法，省略这些方法外围的括号。所有其他的方法调用则使用括号

	```ruby
	class Person
	  attr_reader(:name, :age)  # 差
	  attr_reader :name, :age   # 好
	end
	
	temperance = Person.new 'Temperance', 30  # 差
	temperance = Person.new('Temperance', 30) # 好
	
	puts(temperance.age)  # 差
	puts temperance.age   # 好
	
	x = Math.sin y  # 差
	x = Math.sin(y) # 好
	
	array.delete e  # 差
	array.delete(e) # 好
	
	expect(bowling.score).to eq 0   # 差
	expect(bowling.score).to eq(0)  # 好
	```

* 对于可选参数的哈希，省略其外围的花括号
	
	```ruby
	# 差
	user.set({ name: 'John', age: 45, permissions: { read: true } })
	
	# 好
	user.set(name: 'John', age: 45, permissions: { read: true })
	```

* 对于 DSL 的内部方法调用，同时省略其外围的圆括号与花括号

	```ruby
	class Person < ActiveRecord::Base
	  # 差
	  validates(:name, { presence: true, length: { within: 1..10 } })
	
	  # 好
	  validates :name, presence: true, length: { within: 1..10 }
	end
	```

* 无参调用方法时，省略括号

	```ruby
	# 差
	Kernel.exit!()
	2.even?()
	fork()
	'test'.upcase()
	
	# 好
	Kernel.exit!
	2.even?
	fork
	'test'.upcase
	```


* 当被调用方法是当前区块中唯一操作时，倾向使用简短的传参语法

	```ruby
	# 差
	names.map { |name| name.upcase }
	
	# 好
	names.map(&:upcase)
	```

* 对于单行主体，倾向使用 {...} 而不是 do...end。对于多行主体，避免使用 {...}

	```ruby
	names = %w(Bozhidar Steve Sarah)
	
	# 差
	names.each do |name|
	  puts name
	end
	
	# 好
	names.each { |name| puts name }
	
	# 差
	names.select do |name|
	  name.start_with?('S')
	end.map { |name| name.upcase }
	
	# 好
	names.select { |name| name.start_with?('S') }.map(&:upcase)
	```

* 避免在不需要的情况下使用`self`

	```ruby
	# 差
	def ready?
	  if self.last_reviewed_at > self.last_updated_at
	    self.worker.update(self.content, self.options)
	    self.status = :in_progress
	  end
	  self.status == :verified
	end
	
	# 好
	def ready?
	  if last_reviewed_at > last_updated_at
	    worker.update(content, options)
	    self.status = :in_progress
	  end
	  status == :verified
	end
	```

* 永远不要在方法名与左括号之间添加空格

	```ruby
	# 差
	f (3 + 2) + 1
	
	# 好
	f(3 + 2) + 1
	```

* 未被使用的区块参数或局部变量，添加`_`前缀或直接使用`_`（尽管表意性略差）
	* 这种做法可以抑制 Ruby 解释器或 RuboCop 等工具发出“变量尚未使用”的警告

	```ruby
	# 差
	result = hash.map { |k, v| v + 1 }
	
	def something(x)
	  unused_var, used_var = something_else(x)
	  # ...
	end
	
	# 好
	result = hash.map { |_k, v| v + 1 }
	
	def something(x)
	  _unused_var, used_var = something_else(x)
	  # ...
	end
	
	# 好
	result = hash.map { |_, v| v + 1 }
	
	def something(x)
	  _, used_var = something_else(x)
	  # ...
	end
	```

* 倾向使用 sprintf 或其别名 format 而不是相当晦涩的 String#% 方法

	```ruby
	# 差
	'%d %d' % [20, 10]
	# => '20 10'
	
	# 好
	sprintf('%d %d', 20, 10)
	# => '20 10'
	
	# 好
	sprintf('%{first} %{second}', first: 20, second: 10)
	# => '20 10'
	
	format('%d %d', 20, 10)
	# => '20 10'
	
	# 好
	format('%{first} %{second}', first: 20, second: 10)
	# => '20 10'
	```

* 倾向使用`Array#join`而不是相当晦涩的带字符参数的`Array#*`方法

	```ruby
	# 差
	%w(one two three) * ', '
	# => 'one, two, three'
	
	# 好
	%w(one two three).join(', ')
	# => 'one, two, three'
	```

* 当你希望处理的变量类型是数组，但不太确定其是否真的是数组时，通过使用`[*var]`或`Array()`来替代显式的数组类型检查与转换

	```ruby
	# 差
	paths = [paths] unless paths.is_a? Array
	paths.each { |path| do_something(path) }
	
	# 好
	[*paths].each { |path| do_something(path) }
	
	# 好 - 并且更具可读性
	Array(paths).each { |path| do_something(path) }
	```

* 通过使用范围或`Comparable#between?`来替代复杂的比较逻辑
	
	```ruby
	# 差
	do_something if x >= 1000 && x <= 2000
	
	# 好
	do_something if (1000..2000).include?(x)
	
	# 好
	do_something if x.between?(1000, 2000)
	
	```
* 永远不要使用 END 区块。使用 Kernel#at_exit 来替代

	```ruby
	# 差
	END { puts 'Goodbye!' }
	
	# 好
	at_exit { puts 'Goodbye!' }
	```


* 不要使用`count`作为`size`的替代方案。除了`Array`外，其他`Enumerable`对象都需要通过枚举整个集合才可以确定数目

	
	```ruby
	# 差
	some_hash.count
	
	# 好
	some_hash.size
	```


```ruby

```

## 注释
>良好的代码自身就是最佳的文档。当你要添加一个注释时，扪心自问，“如何改善代码让它不需要注释？” 改善代码，再写相应文档使之更清楚。
——Steve McConnell

>好的代码就像是好的笑话 —— 它不需要解释。
——Russ Olsen

>避免替烂代码编写注释，重构它们使其变得一目了然。(要么做，要么不做，不要只是试试看。——Yoda)

* 很认真的讲：编写让人一目了然的代码然后忽略这一节的其它部分
* 使用英文编写注释
* 前导 # 与注释文本之间应当添加一个空格
* 注释超过一个单词时，句首字母应当大写，并在语句停顿或结尾处使用标点符号。句号后添加一个空格
* 避免无谓的注释

	```ruby
	# 差
	counter += 1 # Increments counter by one.
	```
* 及时更新注释，因为过时的注释比没有注释还要糟糕

### 注解
* 注解应该直接写在相关代码之前那行
* 注解关键字后面，跟着一个冒号及空格，接着是描述问题的文本
* 如果需要用多行来描述问题，后续行要放在 # 后面并缩排两个空格
	
	```ruby
	def bar
	  # FIXME: This has crashed occasionally since v3.2.1. It may
	  #   be related to the BarBazUtil upgrade.
	  baz(:quux)
	end
	
	class ApplicationController < ActionController::Base
	  # Prevent CSRF attacks by raising an exception.
	  # For APIs, you may want to use :null_session instead.
	  protect_from_forgery with: :exception
	end
	```

* 使用 TODO 标记应当加入的特征与功能

* 使用 FIXME 标记需要修复的代码

* 使用 OPTIMIZE 标记可能引发性能问题的低效代码

* 使用 HACK 标记代码异味，即那些应当被重构的可疑编码习惯

* 使用 REVIEW 标记需要确认与编码意图是否一致的可疑代码。比如，`REVIEW: Are we sure this is how the client does X currently?`

* 适当情况下，可以自行定制其他注解关键字，但别忘记在项目的 README 或类似文档中予以说明



## 类与模块
* 在类定义中，使用一致的结构

	```ruby
	class Person
	  # 首先是 extend 与 include
	  extend SomeModule
	  include AnotherModule
	
	  # 内部类
	  CustomErrorKlass = Class.new(StandardError)
	
	  # 接着是常量
	  SOME_CONSTANT = 20
	
	  # 接下来是属性宏
	  attr_reader :name
	
	  # 跟着是其他宏（如果有的话）
	  validates :name
	
	  # 公开的类方法接在下一行
	  def self.some_method
	  end
	
	  # 初始化方法在类方法和实例方法之间
	  def initialize
	  end
	
	  # 跟着是公开的实例方法
	  def some_method
	  end
	
	  # 受保护及私有的方法等放在接近结尾的地方
	  protected
	
	  def some_protected_method
	  end
	
	  private
	
	  def some_private_method
	  end
	end
	```

*  如果嵌套类数目较多，进而导致外围类定义较长，则将它们从外围类中提取出来，分别放置在单独的以嵌套类命名的文件中，并将文件归类至以外围类命名的文件夹下

	```ruby
	# 差
	
	# foo.rb
	class Foo
	  class Bar
	    # 定义 30 多个方法
	  end
	
	  class Car
	    # 定义 20 多个方法
	  end
	
	  # 定义 30 多个方法
	end
	
	# 好
	
	# foo.rb
	class Foo
	  # 定义 30 多个方法
	end
	
	# foo/bar.rb
	class Foo
	  class Bar
	    # 定义 30 多个方法
	  end
	end
	
	# foo/car.rb
	class Foo
	  class Car
	    # 定义 20 多个方法
	  end
	end
	```

* 定义只有类方法的数据类型时，倾向使用模块而不是类。只有当需要实例化时才使用类

	```ruby
	# 差
	class SomeClass
	  def self.some_method
	    # 省略主体
	  end
	
	  def self.some_other_method
	    # 省略主体
	  end
	end
	
	# 好
	module SomeModule
	  module_function
	
	  def some_method
	    # 省略主体
	  end
	
	  def some_other_method
	    # 省略主体
	  end
	end
	```

* 当你想将模块的实例方法变成类方法时，倾向使用`module_function` 而不是`extend self`
	
	```ruby
	# 差
	module Utilities
	  extend self
	
	  def parse_something(string)
	    # 做一些事情
	  end
	
	  def other_utility_method(number, string)
	    # 做一些事情
	  end
	end
	
	# 好
	module Utilities
	  module_function
	
	  def parse_something(string)
	    # 做一些事情
	  end
	
	  def other_utility_method(number, string)
	    # 做一些事情
	  end
	end
	```

* 当设计类的层次结构时，确保它们符合里[式替换原则](https://en.wikipedia.org/wiki/Liskov_substitution_principle)


* 尽量让类做到SOLID(Single responsibility, Open-closed, Liskov substitution, Interface segregation and Dependency inversion)

* 总是替那些用以表示领域模型的类提供一个适当的`to_s`方法

	```ruby
	class Person
	  attr_reader :first_name, :last_name
	
	  def initialize(first_name, last_name)
	    @first_name = first_name
	    @last_name = last_name
	  end
	
	  def to_s
	    "#{@first_name} #{@last_name}"
	  end
	end
	```

* 使用`attr`系列方法来定义琐碎的存取器或修改器

	```ruby
	# 差
	class Person
	  def initialize(first_name, last_name)
	    @first_name = first_name
	    @last_name = last_name
	  end
	
	  def first_name
	    @first_name
	  end
	
	  def last_name
	    @last_name
	  end
	end
	
	# 好
	class Person
	  attr_reader :first_name, :last_name
	
	  def initialize(first_name, last_name)
	    @first_name = first_name
	    @last_name = last_name
	  end
	end
	```

* 避免使用`attr`，使用`attr_reader`与`attr_accessor`来替代
	
	```ruby
	# 差 - 创建单个存取方法（此方法在 Ruby 1.9 之后被移除了）
	attr :something, true
	attr :one, :two, :three # 类似于 attr_reader
	
	# 好
	attr_accessor :something
	attr_reader :one, :two, :three
	```

* 优先考虑使用`Struct.new`。它替你定义了那些琐碎的访问器、构造器及比较操作符

	```ruby
	# 好
	class Person
	  attr_accessor :first_name, :last_name
	
	  def initialize(first_name, last_name)
	    @first_name = first_name
	    @last_name = last_name
	  end
	end
	
	# 更好
	Person = Struct.new(:first_name, :last_name) do
	end
	```

* 优先考虑通过工厂方法的方式创建某些具有特定意义的实例对象

	```ruby
	class Person
	  def self.create(options_hash)
	    # 省略主体
	  end
	end
	```
* 避免使用类变量`@@`。类变量在继承方面存在令人生厌的行为

	```ruby
	class Parent
	  @@class_var = 'parent'
	
	  def self.print_class_var
	    puts @@class_var
	  end
	end
	
	class Child < Parent
	  @@class_var = 'child'
	end
	
	Parent.print_class_var # => 此处打印的结果为 'child'
	```

* 把 `public` `protected` `private`与其作用的方法缩排在同一层级。且在其上下各留一行以强调此可见级别作用于之后的所有方法

	```ruby
	class SomeClass
	  def public_method
	    # ...
	  end
	
	  private
	
	  def private_method
	    # ...
	  end
	
	  def another_private_method
	    # ...
	  end
	end
	```

* 使用`def self.method`定义类方法。这种做法使得在代码重构时，即使修改了类名也无需做多次修改

	```ruby
	class TestClass
	  # 差
	  def TestClass.some_method
	    # 省略主体
	  end
	
	  # 好
	  def self.some_other_method
	    # 省略主体
	  end
	
	  # 在需要定义多个类方法时，另一种便捷写法
	  class << self
	    def first_method
	      # 省略主体
	    end
	
	    def second_method_etc
	      # 省略主体
	    end
	  end
	end
	```

## 异常
*  对于异常处理，倾向使用`raise`而不是`fail`

	```ruby
	# 差
	fail SomeException, 'message'
	
	# 好
	raise SomeException, 'message'
	```

* 不要在带双参数形式的`raise`方法中显式指定`RuntimeError`

	```ruby
	# 差
	raise RuntimeError, 'message'
	
	# 好 - 默认就是 RuntimeError
	raise 'message'
	```

* 倾向使用带异常类、消息的双参数形式调用`raise`方法，而不是使用异常的实例

	```ruby
	# 差 - 并无 raise SomeException.new('message') [, backtraces] 这种调用形式
	raise SomeException.new('message')
	
	# 好 - 与调用形式 raise SomeException [, 'message' [, backtraces]] 保持一致
	raise SomeException, 'message'
	```

* 永远不要从`ensure`区块返回  
	* 如果你显式地从 ensure 区块返回，那么其所在的方法会如同永远不会发生异常般的返回。事实上，异常被默默地丢弃了

	```ruby
	def foo
	  raise
	ensure
	  return 'very bad idea'
	end
	```

* 尽可能隐式地使用`begin/rescue/ensure/end`区块

	```ruby
	# 差
	def foo
	  begin
	    # 主逻辑
	  rescue
	    # 异常处理逻辑
	  end
	end
	
	# 好
	def foo
	  # 主逻辑
	rescue
	  # 异常处理逻辑
	end
	```


* 不要抑制异常

	```ruby
	# 差
	begin
	  # raise exception
	rescue SomeError
	  # do nothing
	end
	
	# 差
	do_something rescue nil
	```

* 避免使用`rescue`修饰语法

	```ruby
	# 差 - 这里将会捕捉 StandardError 及其所有子孙类的异常
	read_file rescue handle_error($!)
	
	# 好 - 这里只会捕获 Errno::ENOENT 及其所有子孙类的异常
	def foo
	  read_file
	rescue Errno::ENOENT => ex
	  handle_error(ex)
	end
	```

* 不要将异常处理作为流程控制使用

	```ruby
	# 差
	begin
	  n / d
	rescue ZeroDivisionError
	  puts 'Cannot divide by 0!'
	end
	
	# 好
	if d.zero?
	  puts 'Cannot divide by 0!'
	else
	  n / d
	end
	```

* 当捕获多个异常时，捕获的顺序按照异常的基层结构从子类到基类。
	* 因为如果把某个异常A的父类放在处理链的较上层，异常A的捕获逻辑将永远不会执行
	
	```ruby
	# 差
	begin
	  # 抛出异常
	rescue StandardError => e
	  # 处理异常
	rescue IOError => e
	  # 处理异常，但事实上永远不会被执行
	end
	
	# 好
	begin
	  # 抛出异常
	rescue IOError => e
	  # 处理异常
	rescue StandardError => e
	  # 处理异常
	end
	```

* 在`ensure`区块释放程序的外部资源

	```ruby
	f = File.open('testfile')
	begin
	  # .. 文件操作
	rescue
	  # .. 处理异常
	ensure
	  f.close if f
	end
	```

* 在调用资源获取方法时，尽量使用具备自动清理功能的版本
	
	```ruby
	# 差 - 需要显式关闭文件描述符
	f = File.open('testfile')
	  # ...
	f.close
	
	# 好 - 文件描述符会被自动关闭
	File.open('testfile') do |f|
	  # ...
	end
	```

* 倾向使用标准库中的异常类而不是引入新的类型


## 集合
 * 对于数组与哈希，倾向使用字面量语法来构建实例（除非你需要给构造器传递参数）
	 
	 ```ruby
	 # 差
	arr = Array.new
	hash = Hash.new
	
	# 好
	arr = []
	hash = {}
	```

* 避免在数组与哈希的字面量语法的最后一个元素之后添加逗`,`，尤其当元素没有分布在同一行时

	```ruby
	# 差 - 尽管移动、新增、删除元素颇为方便，但仍不推荐这种写法
	VALUES = [
	           1001,
	           2020,
	           3333,
	         ]
	
	# 差
	VALUES = [1001, 2020, 3333, ]
	
	# 好
	VALUES = [1001, 2020, 3333]
	```	

* 避免在数组中创造巨大的间隔

	```ruby
	arr = []
	arr[100] = 1 # 现在你有一个很多 nil 的数组
	```

* 当访问数组的首元素或尾元素时，倾向使用`first`或`last`而不是`[0]`或`[-1]`

* 当处理的对象不存在重复元素时，使用`Set`来替代`Array`。`Set`是实现了无序且无重复元素的集合类型。它兼具` Array`的直观操作与`Hash`的快速查找

* 倾向使用符号而不是字符串作为哈希键。 [link]

	```ruby
	# 差
	hash = { 'one' => 1, 'two' => 2, 'three' => 3 }
	
	# 好
	hash = { one: 1, two: 2, three: 3 }
	```

* 避免使用可变对象作为哈希键

* 当哈希键为符号时，使用 Ruby 1.9 的字面量语法
	
	```ruby
	# 差
	hash = { :one => 1, :two => 2, :three => 3 }
	
	# 好
	hash = { one: 1, two: 2, three: 3 }
	```

* 当哈希键既有符号又有字符串时，不要使用 Ruby 1.9 的字面量语法

	```ruby
	# 差
	{ a: 1, 'b' => 2 }
	
	# 好
	{ :a => 1, 'b' => 2 }
	```

* 倾向使用`Hash#each_key`而不是`Hash#keys.each`，使用`Hash#each_value`而不是`Hash#values.each`


	```ruby
	# 差
	hash.keys.each { |k| p k }
	hash.values.each { |v| p v }
	hash.each { |k, _v| p k }
	hash.each { |_k, v| p v }
	
	# 好
	hash.each_key { |k| p k }
	hash.each_value { |v| p v }
	```

* 当处理应该存在的哈希键时，使用`Hash#fetch`

	```ruby
	heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
	
	# 差 - 如果我们打错了哈希键，则难以发现这个错误
	heroes[:batman] # => 'Bruce Wayne'
	heroes[:supermann] # => nil
	
	# 好 - fetch 会抛出 KeyError 使这个错误显而易见
	heroes.fetch(:supermann)
	```


* 当为哈希键的值提供默认值时，倾向使用`Hash#fetch`而不是自定义逻辑

	```ruby
	batman = { name: 'Bruce Wayne', is_evil: false }
	
	# 差 - 如果仅仅使用 || 操作符，那么当值为假时，我们不会得到预期结果
	batman[:is_evil] || true # => true
	
	# 好 - fetch 在遇到假值时依然可以正确工作
	batman.fetch(:is_evil, true) # => false
	```

* 当提供默认值的求值代码具有副作用或开销较大时，倾向使用 Hash#fetch 的区块形式。 [link]

	```ruby
	batman = { name: 'Bruce Wayne' }
	
	# 差 - 此形式会立即求值，如果调用多次，可能会影响程序的性能
	batman.fetch(:powers, obtain_batman_powers) # obtain_batman_powers 开销较大
	
	# 好 - 此形式会惰性求值，只有抛出 KeyError 时，才会产生开销
	batman.fetch(:powers) { obtain_batman_powers }
	```
* 当需要一次性从哈希中获取多个键的值时，使用`Hash#values_at`

	```ruby
	# 差
	email = data['email']
	username = data['nickname']
	
	# 好
	email, username = data.values_at('email', 'nickname')
	```
* 利用`Ruby 1.9 之后的哈希是有序的`的这个特性

* 当遍历集合时，不要改动它

* 当访问集合中的元素时，倾向使用对象所提供的方法进行访问，而不是直接调用对象属性上的`[n]`方法。这种做法可以防止你在`nil`对象上调用`[]

	```ruby
	# 差
	Regexp.last_match[1]
	
	# 好
	Regexp.last_match(1)
	```



## 字符串
* 倾向使用字符串插值或字符串格式化，而不是字符串拼接

	```ruby
	# 差
	email_with_name = user.name + ' <' + user.email + '>'
	
	# 好
	email_with_name = "#{user.name} <#{user.email}>"
	
	# 好
	email_with_name = format('%s <%s>', user.name, user.email)
	```

* 对于插值表达式，括号内两端不要添加空格
	
	```ruby
	# 差
	"From: #{ user.first_name }, #{ user.last_name }"
	
	# 好
	"From: #{user.first_name}, #{user.last_name}"
	```


* 不要使用`?x`字面量语法。在 Ruby 1.9 之后，`?x`与 `'x'`（只包含单个字符的字符串）是等价的

	```ruby
	# 差
	char = ?c
	
	# 好
	char = 'c'
	```

* 不要忘记使用`{}`包裹字符串插值中的实例变量或全局变量

	```ruby
	class Person
	  attr_reader :first_name, :last_name
	
	  def initialize(first_name, last_name)
	    @first_name = first_name
	    @last_name = last_name
	  end
	
	  # 差 - 语法正确，但略显笨拙
	  def to_s
	    "#@first_name #@last_name"
	  end
	
	  # 好
	  def to_s
	    "#{@first_name} #{@last_name}"
	  end
	end
	
	$global = 0
	
	# 差
	puts "$global = #$global"
	
	# 好
	puts "$global = #{$global}"
	```
* 在字符串插值中，不要显式调用`Object#to_s`方法，Ruby 会自动调用它

	```ruby
	# 差
	message = "This is the #{result.to_s}."
	
	# 好
	message = "This is the #{result}."
	```

*  当你需要构造巨大的数据块时，避免使用`String#+`，使用`String#<<`来替代
	* `String#<<`通过修改原始对象进行拼接工作，其比`String#+`效率更高，因为后者需要产生一堆新的字符串对象
	
	```ruby
	# 差
	html = ''
	html += '<h1>Page title</h1>'
	
	paragraphs.each do |paragraph|
	  html += "<p>#{paragraph}</p>"
	end
	
	# 好 - 并且效率更高
	html = ''
	html << '<h1>Page title</h1>'
	
	paragraphs.each do |paragraph|
	  html << "<p>#{paragraph}</p>"
	end
	```

## 正则表达式
>有些人在面对问题时，不经大脑便认为，“我知道，这里该用正则表达式”。现在他要面对两个问题了。
——Jamie Zawinski

* 如果只是在字符串中进行简单的文本搜索，不要使用正则表达式，比如`string['text']`



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

