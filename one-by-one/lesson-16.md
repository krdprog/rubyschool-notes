### Урок 16

### Методы:
public - публичные (обычные)
protected
private

### class API

#### Private - можно вызывать только из методов класса, но не отдельно (ограничивает доступ только внутри класса):

```ruby
class Animal

	attr_reader :name

	def initialize(name)
		@name = name
	end

	def jump
		eat
		puts "#{name}: I am jumping..."
	  sleep
  end

	private

	def eat
		puts "#{name}: I am eating..."
	end
  
  def sleep
		puts "I am sleeping..."
	end
end

cat = Animal.new "Murzik"
cat.jump
# cat.eat # будет ошибка
# cat.sleep # тоже ошибка
```
#### Protected - ... ???

### Статические методы - slef

```ruby
class Man
	def say_hi
		puts "Hi!"
	end
end

man = Man.new
man.say_hi

# or

class Man
	def self.say_hi # !!!
		puts "Hi!"
	end
end

Man.say_hi
```


### yield - вызов

```ruby
def run_5_times
	5.times do
		yield
	end
end

run_5_times { puts "Foooo!!!" }
```

#### Можно передать параметр:

```ruby
def run_5_times
	x = 0
	while x < 5
		yield x
		x += 1
	end
end

run_5_times { |i| puts "Foo! Index: #{i}" }
```
#### Можно передать несколько параметров:
```ruby
def run_5_times
	x = 0
	while x < 5
		yield x, 55
		x += 1
	end
end

run_5_times { |i, v| puts "Foo! Index: #{i}. Value: #{v}" }
```

### Лямбда (lambda) - это указатель на фунцию:

```ruby
x = lambda { #some code }

x = lambda do
  #some code
end

x.call # вызов
```
#### Параметры в labbda-выражениях:

```ruby
x = lambda { |a| puts "#{a}" }

x.call 55
```
```ruby
x = lambda { |a, b| puts "#{a+b}" }

x.call 3, 22
```
#### Работа с массивами:
```ruby
say_hi = lambda { puts "Hi!" }
say_bye = lambda { puts "Bye!" }

week = [ say_hi, say_hi, say_hi, say_hi, say_hi, say_bye, say_bye ]

week.each do |f|
	f.call
end
```
return, но можно не писать:

```ruby
sub_10 = lambda do |x|
	return x - 10
end

a = sub_10.call 1000
puts a
```
```ruby
sub_10 = lambda { |x| return x - 10 }
```

lambda и работа с хешем:

```ruby
# прибавляет 10
# прибавляет 20
# отнимает 5

add_10 = lambda { |x| x+10 }
add_20 = lambda { |x| x+20 }
sub_5 = lambda { |x| x-5 }

# если до 300 - прибавлять 10
# если до 600 - прибавлять 20
# если больше 600 - вычитать 5

balance = 1000

hh = { 111 => add_10, 222 => add_10, 333 => add_20, 444 => add_20, 555 => add_20, 666 => sub_5, 777 => sub_5 }

while true
	x = rand(100..999)
	puts "Combination: #{x}"

	if hh[x]
		f = hh[x]
		balance = f.call balance
		puts "Lambda called."
	else
		balance = sub_5.call balance
	end

	puts "Balance: #{balance}"
	puts "Press Enter to continue..."
	gets
end
```

### Module - модули

> module = namespace = пространство имён

```ruby
module Foo
  class Bar
    #somecode
  end
end

baz = Foo::Bar.new
```
Пример:
```ruby
module Humans

	class Manager
		def say_hi
			puts "Hello!"
		end
	end

	class Hipster
		def say_hi
			puts "Yoooouuuuu! Hi!"
		end
	end

	class Alisha
		def say_hi
			puts "Nihau!"
		end
	end

end

hipster = Humans::Hipster.new
hipster.say_hi
```

#### Встраивание модуля в класс
```ruby
class Animal
  def say
    # some code
  end
end

class Cat
  include Animal
  def eat
    #some code
  end
end
```

#### Mixins

```ruby
module A
   def a1
   end
   def a2
   end
end
module B
   def b1
   end
   def b2
   end
end

class Sample
include A
include B
   def s1
   end
end

samp = Sample.new
samp.a1
samp.a2
samp.b1
samp.b2
samp.s1
```

#### Подключение модуля из другого файла:
```ruby
require './foo.rb'
```

#### Типы переменных: 
```ruby
CONSTANT = 3.14 # константа
$global_var # глобальная переменная
@instance_var # переменная экземпляра
@@class_var # переменная класса (сохраняет своё значение во всех экземплярах класса)
local_var # переменная доступная внутри метода
```

```ruby
class Song
  @@plays = 0
  
  def play
    @@plays += 1
  end
  
  def total_plays
    puts @@plays
  end
end

song1 = Song.new
song2 = Song.new
song3 = Song.new

song1.play
song2.play
song3.play
song3.total_plays
```