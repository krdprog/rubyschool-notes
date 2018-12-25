### Урок 17

#### Метапрограммирование

Функция send - отправить в какую-то функцию какие-то аргрументы.

```ruby
def mm
  #some code
end

# варианты вызова метода:
mm

# metaprogramm:
send :mm
# or
send "mm"
```
#### Параметры в send:
```ruby
def mm param
	puts "#{param}"
end

send :mm, 343
```
#### Передача хеша в send в виде параметра

```ruby
def mm hash
	hash.each { |key, value| puts "#{key} -- #{value}"}
end

send :mm, :foo => 22, :bar => 44
```

```ruby
class Foo
	attr_accessor :name

	def initialize
		send("name=", "Mike") # аналог @name = "Mike"
	end
end

bar = Foo.new
puts bar.name
```

```ruby
class Foo
	attr_accessor :x, :y

	def initialize hash
		hash.each do |key, value|
			send "#{key}=", value
		end
	end

end

s = Foo.new :x => 1, :y => 5

puts s.x
puts s.y
```

```ruby
class Foo

	attr_accessor :name, :age, :country

	def initialize hash
		hash.each do |key, value|
			send("#{key}=", value)
		end
	end

end

bar = Foo.new :name => "Mike", :age => 60, :country => "USA"

p bar
```

### method_missing - метод о несуществовании метода

```ruby
class Foo
	def method_missing name
		puts "Метода #{name} не существует"
	end
end

bar = Foo.new
bar.fjsdahuefd
```

Ещё пример:
```ruby
class Albu
	def initialize(actions)
		@actions = actions
	end

	def method_missing name
		value = @actions[name]
		puts "If you want to #{name}, you must call #{value}."
	end
end

a = Albu.new :cook => "Joe", :take_a_ride => "Jessie", :die => "Gus"
a.cook
a.take_a_ride
a.die
```
> ИЗУЧИ ССЫЛКУ http://www.tutorialspoint.com/ruby/ruby_modules.htm

### Немного хитростей и приёмов:

Лежит ли число в определённом диапазоне чисел:
```ruby
a = 12
a.between?(10, 22)
```
Зацикленный вывод:
```ruby
a = ["a", "b", "c"]
a.cycle { |x| puts x }
```
#### ruby - разбить длинную строку можно указав обратный слэш:

```ruby
puts ' ... ' \
' ... '
```

> rubymine - заменить имя переменной сразу везде: выделить и нажать shift+f6


#### Обнаружение в массиве (метод detect - первый, select - подмассив найденных):

```ruby
arr = [1, 2, 4, 6, 12, 2]

arr.detect { |x| x == 4 }
arr.detect { |x| x < 7 }
arr.select { |x| x < 7 }
```

#### Метод .map

```ruby
arr.map { |x| x*2 }
```

### define_method
```ruby
send :define_method, "aaa" do
	puts "Hello, I`m new method"
end

aaa

# аналог
# def aaa
#   ...
# end
```

```ruby
print "Name of method to define: "
method_name = gets.strip

send :define_method, method_name do
	puts "Hello, I`m new method"
end

send method_name
```

> Преимущество такого подхода в том, что метод вызывается по его имени.

```ruby
def left
	puts "Robot goes left"
end

def right
	puts "Robot goes right"
end

print "Enter your method call: "
my_method = gets.strip

send my_method
```
#### В параметрах к методу хеш должен ставиться последним, т.к. мы не знаем количество элементов хеша.

### split - строку в массив:
```ruby
a = "January, 10, Bob, Alisha, Mike, 44"
p a.split(",")
```
### Разберём текст по буквам:

```ruby
a = "Hello How are you"
p a.split("")
```
### По словам:
```ruby
a = "Hello How are you"
p a.split(" ")
```
#### Количество уникальных букв:
```ruby
a = "Hello How are you"
p a.split("").uniq.size
```

### Работа с файлами:

```ruby
# чтение из файла
input = File.open("test.txt", "r")
puts input.read


# запись в файл
output = File.open("output.txt", "w")

output.write "Foo Bar Baz!"
output.close
```

#### Режимы работы с файлами:
- r - читать
- w - писать
- r+
- w+ - оба: читать, писать
- a - append, добавить в конец файла

#### Чтение файла строка за строкой:
```ruby
# чтение файла строка за строкой:
input = File.open("test.txt", "r")

n = 1

while line = input.gets
	puts "#{n} #{line}"
	n += 1
end

# or
while line = input.gets
	puts line
end
```

#### Решение задачи с чтением файла и подсчётом суммы:

```task.txt
Январь,100
Февраль,20
Март,30
Апрель,20
Май,300
Июнь,100
Июль,22
Август,50
Сентябрь,40
Октябрь,50
Ноябрь,80
Декабрь,0
```
```ruby
input = File.open("task.txt", "r")

total = 0

while line = input.gets
	total += line.split(",")[1].to_i
end

input.close

puts ""
puts "Total: #{total}"
```

#### Переименование файла:
```ruby
File.rename("file.txt", "file_foo.txt")
```

#### chmod в Ruby
```ruby
File.chmod(0644, "/foo.txt", "out")
```

```ruby
puts Dir.pwd
```

cd в Ruby:
```ruby
Dir.chdir '/home/alex/'
```

Массив всех каталогов и файлов по заданному пути:
```ruby
Dir.entries '/foo/bar/'
```

> ДЗ: написать программу которая ищет файл на диске

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md