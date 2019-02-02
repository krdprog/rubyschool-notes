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