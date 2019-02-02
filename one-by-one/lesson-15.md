## Урок 15

#### Атрибуты (свойства класса):

```ruby
attr_reader
attr_writer
attr_accessor
```

```ruby
class Some
  attr_writer :foo
end

bar = Some.new
bar.foo = 12
puts bar.foo
```

```ruby
class Song
  attr_accessor :name, :duration

  def initialize name, duration
    @name = name
    @duration = duration
  end
end

song1 = Song.new 'Obla-di, obla-da', 6
puts song1.name
```

```ruby
class Airport
  attr_reader :name

  def initialize name
    @name = name
  end
end

class Airplane
  attr_reader :model

  def initialize model
    @model = model
  end
end
```
1 аэропорт содержит много самолётов (*):

#### Задача Аэропорты и самолёты в них:

```ruby
# 1. создать 2 аэропорта с 3 самолётами в каждом
# 2. вывести на экран название аэропортов и какие в нём самолёты

class Airport
  attr_reader :name, :planes

  def initialize(name)
    @name = name
    @planes = []
  end

  def add_plane plane
    @planes << plane
  end
end

class Plane
  attr_reader :model

  def initialize(model)
    @model = model
  end
end

# массив для хранения аэропортов
airports = []

# создадим 2 аэропорта
airport1 = Airport.new "SVO"
airport2 = Airport.new "VKO"

# запишем их в массив аэропортов
airports << airport1
airports << airport2

# add plane in airport1
plane1 = Plane.new "Airbus"
plane2 = Plane.new "Boeing"
plane3 = Plane.new "IL-76"

airport1.add_plane plane1
airport1.add_plane plane2
airport1.add_plane plane3

# add plane in airport2
plane2_1 = Plane.new "DoooDdoo"
plane2_2 = Plane.new "Foofo"
plane2_3 = Plane.new "FafoooFf"
plane2_4 = Plane.new "Booooou"

airport2.add_plane plane2_1
airport2.add_plane plane2_2
airport2.add_plane plane2_3
airport2.add_plane plane2_4

# выведем информацию об аэропортах и самолётах в них:
airports.each do |airport|
  puts "#{airport.name}"

  puts "Planes in this airport:"
  airport.planes.each do |plane|
    puts "#{plane.model}"
  end

  puts "========================="
end
```

#### Вариант задачи Аэропорт-Самолёты со Страной:

```ruby
class Country
  attr_reader :name, :airports

  def initialize(name)
    @name = name
    @airports = []
  end

  def add_airport airport
    @airports << airport
  end
end

class Airport
  attr_reader :name, :planes

  def initialize(name)
    @name = name
    @planes = []
  end

  def add_plane plane
    @planes << plane
  end
end

class Plane
  attr_reader :model

  def initialize(model)
    @model = model
  end
end

# массив стран
countries = []

# создадим 2 страны
russia = Country.new "Russia"
germany = Country.new "Germany"

# добавим в массив стран
countries << russia
countries << germany

# создадим 2 аэропорта
airport1 = Airport.new "MOSCOW"
airport2 = Airport.new "BERLIN"

# добавим аэропорты в страну
russia.add_airport airport1
germany.add_airport airport2

# add plane in airport1
plane1 = Plane.new "Airbus"
plane2 = Plane.new "Boeing"
plane3 = Plane.new "IL-76"

airport1.add_plane plane1
airport1.add_plane plane2
airport1.add_plane plane3

# add plane in airport2
plane2_1 = Plane.new "DoooDdoo"
plane2_2 = Plane.new "Foofo"
plane2_3 = Plane.new "FafoooFf"
plane2_4 = Plane.new "Booooou"

airport2.add_plane plane2_1
airport2.add_plane plane2_2
airport2.add_plane plane2_3
airport2.add_plane plane2_4

# выведем информацию о странах, аэропортах и самолётах в них:

countries.each do |country|
  puts "Airports From #{country.name}:"
  country.airports.each do |airport|
    puts "- Airport #{airport.name}"

      puts "-- Planes in this airport:"
      airport.planes.each do |plane|
        puts "-- #{plane.model}"
      end
  end
  puts "=================="
end
```
#### Задача: альбом с песнями

```ruby
# add album and 3 song

class Album
  attr_reader :name, :songs

  def initialize(name)
    @name = name
    @songs = []
  end

  def add_song song
    @songs << song
  end

end

class Song
  attr_reader :name, :duration

  def initialize(name, duration)
    @name = name
    @duration = duration
  end

end

# add massive with albums
albums = []

abba = Album.new 'Abba'

albums << abba

song1 = Song.new "Foooo Barrrr!", "1:44"
song2 = Song.new "Baz Baz Baz!", "4:30"
song3 = Song.new "Yahooou!", "2:32"

abba.add_song song1
abba.add_song song2
abba.add_song song3

albums.each do |album|
  puts "Album: #{album.name}", ""
  album.songs.each do |song|
    puts "Song: #{song.name} -- duration: #{song.duration}"
  end
end
```

### Наследование (inheritance):

```ruby
class Parent
  def foo
    puts "foo"
  end
end

class Child < Parent
end
```

#### Наследование от суперкласса метода initialize - через super:

```ruby
class Animal
  def initialize(name)
    @name = name
  end

  def jump
    puts "#{@name} is jumping..."
  end
end

class Cat < Animal

  def initialize
    super 'Murzik'
  end

  def say_meow
    puts "#{@name} Meowww!"
  end
end

class Dog < Animal

  def initialize
    super 'Sharik'
  end

  def say_gav
    puts "#{@name} Gavvv!"
  end
end

cat = Cat.new
dog = Dog.new

cat.jump
cat.say_meow

dog.jump
dog.say_gav
```

> изучи ссылку [Classes, Objects, and Variables](http://phrogz.net/programmingruby/tut_classes.html)

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |