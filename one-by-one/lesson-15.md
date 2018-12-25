### Урок 15

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