## Урок 14

#### ООП, Класс, Объект (экземпляр класса, instance)

```ruby
class Animal
  def say
    puts "Foooo!"
  end
end

cat = Animal.new
cat.say #=> Fooooo!
```
Переменные внутри класса:

```ruby
class Animal
  def say
    @name = 'Dog'
    puts "I am #{@name}!"
  end
end
```
Переменные класса в отдельный метод initialize (в других языках это называется - конструктор):
```ruby
def initialize
  @hh = {}
  @param = 200
end
```

#### Задача записная книжка:

```ruby
# класс записной книжки
class Book

  def initialize
    @hh = {}
    @last_person = ''
  end

  def add_person options
    @last_person = options[:name]

    puts "Уже есть!" if @hh[options[:name]]

    @hh[options[:name]] = options[:age]
  end

  def show_all
    @hh.each do |name, age|
      puts "#{name} is #{age} years old."
    end
  end

  def show_last_person
    puts "Last person: #{@last_person}."
  end

end

b = Book.new

b.add_person :name => "Joe", :age => 44
b.add_person :name => "Mark", :age => 14

b.show_all
b.show_last_person
```

```ruby
class Cat
  def initialize
    @foo = 12
  end

  def aaa
    return @foo
  end
end

bar = Cat.new
bar.aaa => 12
  ```
#### attr_reader - делает переменную доступную для чтения:
```ruby
def last_person
  @last_person
end

# заменяется на:
attr_reader :last_person
```

#### attr_accessor - для чтения и записи:
```ruby
attr_accessor :last_person
```

#### Самолёт:
```ruby
class Airplane

  attr_reader :model
  attr_reader :speed
  attr_reader :altitude

  def initialize(model)
    @model = model
    @speed = 0
    @altitude = 0
  end

  def fly
    @speed = 800
    @altitude = 10000
  end

  def land
    @speed = 0
    @altitude = 0
  end

  def moving?
    return @speed > 0
  end

end

plane1 = Airplane.new('Boeing-777')
plane1.fly
puts "Model: #{plane1.model}, Speed: #{plane1.speed}, Altitude: #{plane1.altitude}"
plane1.land
puts "Model: #{plane1.model}, Speed: #{plane1.speed}, Altitude: #{plane1.altitude}"
puts "Is moving: #{plane1.moving?}"
```

#### Самолёт версия 2 - запуск рандомных самолётов:
```ruby
# самолёт
class Airplane

  attr_reader :model
  attr_reader :speed
  attr_reader :altitude

  def initialize(model)
    @model = model
    @speed = 0
    @altitude = 0
  end

  def fly
    @speed = 800
    @altitude = 10000
  end

  def land
    @speed = 0
    @altitude = 0
  end

  def moving?
    return @speed > 0
  end

end

models = ['Il-76', 'Boeing-777', 'Airbus-320']

planes = []

20.times do
  model = models[rand(0..2)]
  plane = Airplane.new(model)

  if rand(0..1) == 1
    plane.fly
  end

  planes << plane

end

planes.each do |plane|
  puts "Model: #{plane.model}, Speed: #{plane.speed}, Altitude: #{plane.altitude}"
  puts "Plane moving: #{plane.moving?}"
end
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |