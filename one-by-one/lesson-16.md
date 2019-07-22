## Урок 16

### Существует 3 уровня доступа к методам. Методы бывают:

- открытый (public) интерфейс — общий интерфейс для всех пользователей данного класса;
- защищённый (protected) интерфейс — внутренний интерфейс для всех наследников данного класса;
- закрытый (private) интерфейс — интерфейс, доступный только изнутри данного класса.

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

### Статические методы - self

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

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |