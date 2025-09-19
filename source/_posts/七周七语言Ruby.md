---
title: 七周七语言：Ruby
date: 2025-07-31
categories:
- 编程语言
tags:
- 七周七语言
- Ruby
excerpt: "七周七语言Ruby部分"
---
## 1 环境配置
### 1.1 windows配置
1. 打开[RubyInstaller](https://rubyinstaller.org/downloads/)，选择`rubyinstaller-devkit-3.4.4-2-x64.exe`，下载。
2. 双击打开程序，选择Accept，点击Next。

    ![](/images/ruby-1.png)

3. 点击Browse安装到自定义位置，不想改直接Install。

    ![](/images/ruby-2.png)

4. 勾选`MSYS2`开发工具链，单机Next。

    ![](/images/ruby-3.png)

5. 下载完成后点击Finish

    ![](/images/ruby-4.png)

6. 会弹出一个命令行窗口，直接按Enter键，等待执行。

    ![](/images/ruby-5.png)

7. 到这一步，Enter键结束安装。
    ![](/images/ruby-6.png)

8. 打开cmd（win键+r，输入cmd），输入`ridk version`，看到类似输出则ridk安装正确。
    ![](/images/ruby-7.png)

9. cmd输入`irb`进入ruby交互式环境，到这里已经可以执行ruby代码了，推荐配合vscode编辑器使用。
### 1.2 MacOS环境配置 
1. 打开终端，安装Homebrew：
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
2. 安装`rbenv`
```bash
brew install rbenv ruby-build
```
3. 判断当前shell并修改配置文件
```bash
echo $SHELL
```
这行代码能判断你当前的shell环境。

- Zsh：
```bash
echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
source ~/.zshrc
```
- Bash：
```bash
echo 'eval "$(rbenv init - bash)"' >> ~/.bash_profile
source ~/.bash_profile
```
4. 安装ruby
```bash
rbenv install 3.2.2
rbenv global 3.2.2
```
验证版本是否正确：
```bash
ruby -v
```
你应该看到 `ruby 3.2.2` 之类的输出。

5. 安装Bundler（Ruby 包管理工具）
```bash
gem install bundler
```

6. 安装irb
```bash
gem install pry
```
终端输入`irb`进入交互环境。
!!! warning
    - 不建议直接使用系统自带的 Ruby（容易与 macOS 系统冲突）。
    - 推荐使用 `rbenv` 来管理 Ruby 版本，便于升级和隔离项目环境。
    - 安装 Ruby 时若遇到 readline 错误，可执行：`brew install openssl readline`。
### 1.3 执行代码
ruby代码分为两种执行方式，**irb执行**和**文件执行**。
简单的命令使用irb，而复杂的代码建议写入一个文件，通过ruby+文件名执行，比如：`ruby hello.rb`。
如果vscode推荐安装Run code插件快速运行。

## 2.第一天：对象与流程控制
!!! abstract
    本章节通过Ruby的对象系统、判断语句和循环结构，初步感受ruby的编程方式。 
### 2.1 起步
输出是学习一门编程语言最基础的一步。
打开irb，输入以下命令：
```rb
irb(main):001> puts "Hello,world"
Hello,world
=> nil
irb(main):002> lang = 'ruby'
=> "ruby"
irb(main):003> puts "Hello #{lang}" #这是模板字符串的用法
Hello ruby
=> nil
```
这三条代码中，传达了很多 Ruby 的核心特性：

- puts 是ruby的输出函数。
- 字符串用单引号 `''` 或双引号 `""` 包裹。
- 变量 **无需声明类型**，可以直接赋值。
- 模板字符串使用 `#{变量}` 的格式，仅在双引号内有效。
- Ruby的每条语句都有返回值。
- `nil` 代表空值。

### 2.2 编程模型
Ruby是一门纯面向对象的语言，在ruby中，一切皆对象。
```ruby
irb(main):001:0> 5.class
=> Integer
irb(main):002:0> 5.methods
=>
[:remainder,
 :abs,
 :magnitude,
 :zero?,
 :floor,
 # skip
 :__id__]
```
在这个例子中，
一个对象具有个最基本的方法：

- obj.class：获取对象所属的类。比如 `5.class` 表示整数 `5` 的类为 `Integer`。
- obj.methods：获取对象所有的方法，以列表返回。


!!! tips ""
    原书中 `4.class` 返回 `Fixnum`，但从 Ruby 2.4 起，`Fixnum` 和 `Bignum` 已被统一为 `Integer`。

### 2.3 判断与循环
#### 布尔值
在 Ruby 中，每条语句都有返回值，所有返回值都可以归类为“真值”或“假值”。
只有 nil 和 false 被视为 假值，其他所有值，包括 0 和空字符串，都被视为 真值。需要注意的是，虽然 0 和其他非 false 或 nil 的值都被视为“真”，但它们并不等同于布尔值 true。
例如，0 == true 会返回 false，因为 0 和 true 是不同的值。

!!! tips "注意!"
    在一些其他语言中，比如 `python`、`C/C++`中，`0` 也是 `false`。而在Ruby中，`0` 是 `true`。
#### 判断语句
- 块形式：
```ruby
if condition
  statment
end
```
- 单行形式：
```ruby
statement if condition 
```
#### 逻辑运算符
- and：两个条件都为true，返回true
- or：两个之中一个为true，返回true
- 逻辑短路：当表达式值已经明确求出就不会执行后面的表达式代码。
??? tip "逻辑短路例子"

    ```ruby
    def a
      puts "调用了a"
      return true
    end

    def b
      puts "调用了b"
      return false
    end

    puts "---&&短路---"
    a && b   # 两个都执行，因为a是true，结果要看b

    puts "---||短路---"
    a || b   # 只执行a，因为a是true，已经能决定结果，不再执行b
    ```
    输出：
    ```
    ---&&短路---
    调用了a
    调用了b
    ---||短路---
    调用了a
    ```
??? tip "逻辑运算符：&& / || 与 & / | 的区别"
    - `&&` 和 `||` 是 **逻辑运算符**，具有**短路行为**：如果前一个条件已经可以决定结果，就不会再执行后一个条件。  
      例如：`false && do_something()` 中，`do_something()` 不会被调用。
    
    - `&` 和 `|` 是 **按位运算符**，**不具备短路特性**，即使前面的条件已决定结果，也会继续执行后面的表达式。  
      同时，`&` 和 `|` **可以被重载**，常用于集合操作、自定义对象的运算符重载等场景。
??? tip "重载"
    所谓“重载”，就是**重定义操作符对应的方法**。比如下面这个例子：
    ```ruby
    class MyObject
        def &(other)
            puts "执行自定义的 & 运算"
        end
        end

        a = MyObject.new
        b = MyObject.new
        a & b   # 输出：执行自定义的 & 运算
    ```
    本质上，&、+、== 等都是方法名，调用 a & b 等价于 a.&(b)。这正是ruby语法糖的设计哲学之一。
#### unless
`unless` 是ruby中特有的条件关键字。
!!! example "书12页例"
    ```ruby
    irb(main):001:0> x = 4
    => 4
    irb(main):002:0> puts "This appears to be false" unless x == 4
    => nil
    irb(main):003:0>
    ```
在这个例子中，如果x不等于4才会输出内容。

通俗的解释，`unless` 可以理解为 `if not`。带入语言的语境就是 “除非x等于4，否则就打印”

#### until循环
和之前的 `unless` 一样，通过单词的语义去理解until循环是最方便的。unless是直到的意思，放在语境中就是“一只做某事直到条件达成”
!!! example "书13页例"
    ```ruby2879
    irb(main):001:0> x = 10
    => 10
    irb(main):002:0> x = x - 1 until x == 1
    => nil
    irb(main):003:0> x
    => 1
    ```
在这个例子中，虽没有显示，但是发生里9次循环，x-=1后判断x是否等于1，如果没有则继续，直到x=1。

#### times循环
`Integer` 的实例有 `times方法`，即循环多少次。
```ruby
irb(main):001:0> 3.times {puts "a"}
a
a
a
=> 3
```
times循环通常配合块使用。

### 2.4 鸭子类型
鸭子类型并不是特定的语法，而是一种编程思想。国外有句谚语：
>"If it walks like a duck and it quacks like a duck, then it probably is a duck."

>如果它走起来像鸭子，叫起来也像鸭子，那它八成就是只鸭子。

在 Ruby 中，对象的“类型”并不是最重要的，是否拥有某些行为（方法）才是关键。换句话说，只要一个对象实现了某个方法，它就可以“扮演”需要这个方法的角色。

这种做法被称为**行为驱动**，是 Ruby 这类动态语言的一大特色。

简而言之，不同的类实实现同一套类的名字就是鸭子方法的本质。
```ruby
class Dog
  def speak
    puts "Woof!"
  end
end

class Duck
  def speak
    puts "Quack!"
  end
end

def make_it_speak(animal)
  animal.speak
end

make_it_speak(Dog.new)   # => Woof!
make_it_speak(Duck.new)  # => Quack!
```
这里的 `make_it_speak` 函数不在乎参数是 `Dog` 还是 `Duck`，只关心你有没有 `speak` 方法。

## 3.第二天：函数，数组与代码块
!!! abstract 
    在第二天的学习中，我们将深入 Ruby 的三大基础组成：函数（方法）、数组（Array） 与 代码块（Block）。它们共同构成了 Ruby 编程的核心操作与风格。
### 3.1 函数
```ruby
def funcname(arg)
  statement
end
funcname(arg) # 调用函数
```
Ruby中函数的定义非常的方便：

- 使用 def 关键字定义方法，以 end 结束。
- 方法名后可以跟参数列表，括号可省略（特别是无参时）。
- 不需要显示使用`return`关键字，函数会返回最后一条代码的值。
- 调用函数时如果没有参数不需要括号。
- 有参数也可与直接写在后面而不用括号。只限于一个参数。

```ruby
def greet(name)
  "Hello, #{name}!"
end

puts greet("Ruby")    # 输出：Hello, Ruby!
puts greet "World"     # 小括号也可以省略
```
>Ruby 语法强调简洁、可读和自然语言风格，这种灵活的函数调用形式正体现了 Ruby 的设计哲学。

### 3.2 数组与切片
数组是一种有序集合，可以存储多个对象，类型可以混合。它类似于 Python 中的 `list`。

- 数组中可以存储不同类型的元素。
```ruby
irb(main):001:0> arr = [1,'two',3.0]
=> [1, "two", 3.0]
```
- `数组名[index]` 访问元素，负数代表从后往前。
```ruby
irb(main):002:0> arr[0]
=> 1
irb(main):003:0> arr[-1]
=> 3.0
```
- `数组名[index] = ''` 通过下表修改元素。
```ruby
irb(main):004:0> arr[0] = 'one'
=> "one"
irb(main):005:0> arr
=> ["one", "two", 3.0]
```
- `<<` 添加元素到末尾。
```ruby
irb(main):006:0> arr << 'four'
=> ["one", "two", 3.0, "four"]
```
- `数组名.push(元素)` 添加元素到末尾。
```ruby
irb(main):007:0> arr.push(5)
=> ["one", "two", 3.0, "four", 5]
```
- `pop` 删除列表最后一个元素并返回这个元素。
```ruby
irb(main):008:0> arr.pop
=> 5
irb(main):009:0> arr
=> ["one", "two", 3.0, "four"]
```
- `数组名[start_inde,end_index]` 数组切片，不包含end_index。
```ruby
irb(main):010:0> arr[0,2]
=> ["one", "two"]
```

### 3.3 散列表与符号
散列表中每个元素是键值对的形式，类似python中的`dict`。
如果键是字符，必须在前面添加冒号，叫做符号（Symble）。
!!! tip "为什么散列表使用符号作为键？"
    在 Ruby 中，使用符号（Symbol）作为散列表的键有几个主要原因：

    - 符号的定义就是一个固定内存地址的标识。
    - 符号在内存中只会存留一个副本，无论在程序中使用多少相同的符号多会指向同一个内存。
    - 符号内存占用非常小


- 散列表通过大括号包裹的形式定义，其中每项对应 `key=>value`
```ruby
irb(main):001:0> user = {:name=>"m310ct",:age=>17}
=> {:name=>"m310ct", :age=>17}
```
- `散列表名[key]` 获取对应的value
```ruby 
irb(main):002:0> puts "User name is #{user[:name]}"
User name is m310ct
=> nil
```
- `散列表[key] = value` 修改对应key的value
```ruby
irb(main):003:0> user[:name] = "m310ctaaa"
=> "m310ctaaa"
irb(main):004:0> user
=> {:name=>"m310ctaaa", :age=>17}
```

散列表还能过模拟命名参数。
!!! example "书20页例"
    ```ruby
    irb(main):001:1* def tell_the_truth(options={})
    irb(main):002:2*   if options[:profession] == :layer
    irb(main):003:2*     'it could be believed that this is almost
    certainly not false.'
    irb(main):004:2*   else
    irb(main):005:2*     true
    irb(main):006:1*   end
    irb(main):007:0> end
    => :tell_the_truth
    irb(main):008:0> tell_the_truth
    => true
    irb(main):009:0> tell_the_truth :profession => :layer
    => "it could be believed that this is almost certainly not false."
    ```
`options`是一个散列表，作为 `tell_the_truth` 函数的参数，来模拟命名参数。判断如果传入的散列表中，`:profession` 对应的是 `:layer` 就返回字符串，否则返回true。这段代码理解起来有几个难点：

- 调用语句 `tell_the_truth :profession => :layer`。之前说过，如果函数只有一个参数可以不加括号，这句代码其实等价于 `tell_the_truth ({:profession => :layer})`

- 散列表通过 `options[:profession]` 来访问键 `:profession` 的值。这里用的是 **符号作为键**，符合 Ruby 的习惯用法（原因参考前文的 Symbol tip）。

### 3.4 代码块与yield
在times循环的例子中已经接触过代码块了。

- 代码块可以理解为一段 **没有名字的函数**。
- 代码块有两种书写形式：
    - 当只有一行代码时用花括号 `{}` 的形式
    - 多行代码用 `do...end`。

```ruby
3.times { puts "Hello" }

3.times do
  puts "Hello"
end
```

#### 块参数
就和函数有参数一样，代码块也可以接受参数，称为 **块参数**。
参数块的形式为 `|变量名|` ，代码块参数的值由调用它的方法在每次执行时传入。
```ruby
irb(main):001:0> [1, 2, 3].each { |i| puts i }
1
2
3
=> [1, 2, 3]
```

#### yield
`yield` 用于调用代码块。
`yield(参数)` 用于传递块参数。
```ruby
irb(main):001:1* def print_name
irb(main):002:1*   yield("m310ct")
irb(main):003:0> end
=> :print_name
irb(main):004:1* print_name do |name|
irb(main):005:1*   puts "Your name is #{name}"
irb(main):006:0> end
Your name is m310ct
=> nil
```
在这个例子中，调用 `print_name` 函数的时候传入一个打印名字的代码块。`print_name` 中yield就相当于调用这个代码块并且给它穿入参数。

#### &block
!!! example "21页例"
```ruby
irb(main):001:1* def call_block(&block)
irb(main):002:1*   block.call
irb(main):003:0> end
=> :call_block
irb(main):004:1* def pass_block(&block)
irb(main):005:1*   call_block(&block)
irb(main):006:0> end
=> :pass_block
irb(main):007:0> pass_block {puts 'Hello,block'}
Hello,block
=> nil
```
这是一个 **代码块传递机制** 的一个经典例子。

1. `call_block` 函数

    ```ruby
    def call_block(&block)
      block.call
    end
    ```

    - `&block` 这个方法接收一个代码块作为参数，并将传进来的代码块封装为一个 `Proc` 对象。
    - `block.call` 执行这个代码块。

    !!! tip "为什么要把代码块转为 Proc 对象？"
        - 在 Ruby 中，代码块并不是一个对象。
        - 代码块无法当作普通变量使用、传来传去、赋值等操作。
        - 如果希望代码块能和普通对象一样使用，就需要用 `&block` 接收，并且用 `.call` 调用。

2. `pass_block` 函数

    ```ruby
    def pass_block(&block)
      call_block(&block)
    end
    ```

    - `pass_block` 也接收一个 `block`（用 `&block`）。
    - 它把这个 `block` 传给另一个方法 `call_block`。
    - 传递的时候也需要加 `&`，否则就是普通对象，`call_block` 不会把它当成 `block` 处理。

3. 调用 `pass_block { puts 'Hello, block' }`

    !!! tip "& 的本质"
        总结一句话：你用 `&` 修饰的参数，在传入 block 时变成 `Proc`，在传入 `Proc` 时又能变回 block，`&` 就是 `block` 和 `Proc` 之间的桥梁。

        举个例子：

        ```ruby
        def outer(&block)
          inner(&block)
        end

        def inner
          puts "准备执行 block"
          yield
          puts "执行完毕"
        end

        outer { puts "我是 block" }
        ```

        - 在 `outer(&block)` 会把 `block` 转为 `Proc`。
        - 在 `inner(&block)` 会把 `Proc` 转为 `block`。
        - 即使方法定义时没有写任何形式参数，只要内部用了 `yield`，并且调用时传入了 `block`，`yield` 就能执行这个 `block`。这也是为什么 `inner` 函数没有写形参却可以执行传入的 block 的原因。


## 4.第三天：类、模块与元编程

### 4.1 定义类
```ruby
class Person
    attr_accessor :name, :age
    
    def initialize(name, age)
        @name = name
        @age = age
    end
    
    def greet
        "Hello, my name is #{@name} and I am #{@age} years old."
    end
end
    
person = Person.new("m310ct", 17)
puts person.greet
```
上面这个例子定义了一个 `Person` 类，实现了一个 `greet` 方法：

- `attr_accessor :name, :age` 的作用是给 `name` 和 `age` 创建了 `getter` 和 `setter` 方法。
??? tips "定义getter和setter方法"
    ```ruby
    class Person
      def name #getter
        @name
      end

      def name=(val) #setter
        @name = val
      end
    end

    p = Person.new
    p.name = ("m310ct") # 调用setter方法
    ```
    `setter` 方法的形式比较怪异，调用方式也比较怪异。
    区别于传统的直接传参数，Ruby 把赋值行为当作“方法调用”。
    
- `initialize` 方法会在类实例化时自动执行，一般在其中定义实例变量。

### 4.2 Mixin和模块
**模块** 是一种用于组织和复用代码的结构，无法实例化。

**Mixin** 是一种代码复用机制，用于将模块中的方法“混入”类中，从而扩展类的功能，而无需继承。

!!! example "书25页例"
    ```ruby
    module ToFile
      def filename
        "object_#{self.object_id}.txt"
      end

      def to_f
        File.open(filename, 'w') {|f| f.write(to_s) }
      end
    end

    class Person
      include ToFile
      attr_accessor :name
      def initialize(name)
        @name = name
      end
      def to_s
        name 
      end
    end

    Person.new("Alice").to_f
    ```
    这段代码的主要功能就是将传入的名字保存为一个txt文件。
    `module ToFile` 是一个创建并写入文件的模块，其中定义了两个方法：
    
    - filename方法：返回特定格式构建好的文件名
    - to_f 方法：创建并写入txt文件

    在 `class Person` 类中，通过 `inculde ToFile` 混入模块，就好像直接在 `Person` 类中写入了 `filename` 和 `to_f` 方法。

    这就是Mixin的体现，`ToFile` 封装了写入文件功能，之后可以复用。

### 4.3 枚举和集合
**枚举** 和 **比较** 是ruby中至关重要的两个Mixin。

- 枚举：要求类实现 `each` 方法
- 比较：要求类实现 `<=>` 太空船操作符。

??? tip "太空船操作符"
    `a <=> b`：快速判断大小的操作符。

    - a 大于 b：返回1
    - a 小于 b：返回-1
    - a 等于 b：返回0


**集合** 实现了很多便于使用的可枚举和课比较的方法。

```ruby 
irb(main):001:0> a = [5,4,3,2,1]
=> [5,4,3,2,1]
```
先创建一个数组，根据例子解释不同的方法：

- `sort`：元素升序排列：
```ruby
irb(main):002:0> a.sort
=> [1, 2, 3, 4, 5]
```

- `any?`：一个元素满足条件返回 `true`：
```ruby
irb(main):003:0> a.any? {|i| i > 3}
=> true
```

- `all?`：全部元素满足条件返回 `true`：
```ruby
irb(main):004:0> a.all? {|i| i > 5}
=> false
```

- `select`：列举所有满足条件的元素，以列表形式返回：
```ruby
irb(main):005:0> a.select {|i| i > 3}
=> [5, 4]
```

- `collect` 和 `map`：对每个元素执行操作：
```ruby
irb(main):007:0> a.collect {|i| i += 1}
=> [6, 5, 4, 3, 2]
```

- `inject` 用于计算元素的和或积：
```ruby
irb(main):008:0> a.inject {|s,i| s = s+i}
=> 15
irb(main):010:0> a.inject {|s,i| s = s*i}
=> 120
irb(main):013:0> a.inject(:+)
=> 15
irb(main):014:0> a.inject(:*)
=> 120
```

### 4.4 元编程
**开放类** 允许在任何时候重新打开已经存在的类并且修改其方法。
```ruby
class Integer
  def to_chinese
    ["零", "一", "二", "三", "四", "五", "六", "七", "八", "九"][self]
  end
end

puts 3.to_chinese   # 输出: 三
puts 8.to_chinese   # 输出: 八
```
这个例子就是给 `Integer` 类新添加一个方法，使数字能方便的转换为中文。

#### method_missing
当ruby找不到某个方法的时候就会调用 `method_missing` 方法。可以人为重写这个方法来实现自定义功能。
通过 `self.method_missing`定义。有三个形参，分别是未找到的方法名、参数和代码块。
```ruby
class Test
    def self.method_missing(method_name, *args, &block)
        puts "method_missing called for #{method_name}"
    end
end

Test.qqq  #调用一个不存在的方法会调用method_missing
```

#### 元编程和常用方法
元编程就是**程序写程序**，简而言之即允许在运行时创建/删除/修改类、方法、变量，像魔术一样操作代码本身。

```ruby
class User
    [:name,:age].each do |args|
        define_method("get_#{args}") do
            instance_variable_get("@#{args}")
        end

        define_method("set_#{args}") do |value|
            instance_variable_set("@#{args}",value)
        end
    end
end

u = User.new
u.set_age(10)
puts u.get_age
```
这段代码中，动态的创建了name和age两个实例变量并且依次创建了 `get` 和 `set` 方法。
常用元编程方法：

| 方法名                            | 说明                          |
| ------------------------------ | --------------------------- |
| `define_method(name, &block)`  | 动态定义实例方法                    |
| `instance_variable_get/set`    | 读取或设置实例变量（通过字符串或 Symbol 访问） |
| `send(:method_name, *args)`    | 动态调用方法（包括私有方法）              |
| `method_missing(name, *args)`  | 拦截未定义方法的调用，常用于实现动态行为或 DSL   |
| `define_singleton_method`      | 为某个对象单独定义方法，不影响类中其他对象       |
| `class_eval` / `instance_eval` | 在类或对象上下文中动态执行代码，常用于修改行为     |
