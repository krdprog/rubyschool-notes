## Урок 17

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

- **r** - режим **"только для чтения"**: указатель файла находится в начале файла. Это режим "по-умолчанию". Если файла не существует - выпадет ошибка.
- **w** - режим **"только для записи"**: перезаписывает файл, если файл существует. Если файл не существует, создаёт новый файл для записи.
- **r+** - режим **"чтения и записи"**: указатель файла будет в начале файла. При осуществлении записи будет затирать уже существующие строки в начале файла. Если файла не существует - выпадет ошибка.
- **w+** - режим **"чтения и записи"**: перезаписывает существующий файл, если файл существует. Если файл не существует, создаёт новый файл для чтения и записи.
- **a** - append, добавить в конец файла. Режим **"только для записи"**. Указатель файла находится в конце файла, если файл существует. То есть файл находится в режиме добавления. Если файл не существует, он создаёт новый файл для записи.
- **a+** - режим **"чтения и записи"**. Указатель файла находится в конце файла, если файл существует. Файл открывается в режиме добавления. Если файл не существует, он создаёт новый файл для чтения и записи.

#### Про режим a+

text.txt:
```txt
Foo!
Bar!
Baz!
```

app.rb
```ruby
file = File.open("text.txt", "a+") # откроем файл для добавления и чтения
puts file.read # прочитаем содержимое файла

file.write "Karamba!" # добавим в файл

file.rewind
puts file.read # прочитаем ещё раз файл

file.close # закроем файл
```

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

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |