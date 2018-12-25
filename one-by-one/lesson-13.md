### Урок 13

#### Ещё вариант добавления в хеш:

```ruby
hh = {}
hh.store('Mike', 65)
```

```ruby
hh = {'mike' => 10, 'alisha' => 42}

hh.keys.each do |key|
  hh[key] = 100
end

puts hh

#=> mike 100, alisha 100
```

#### Очистить хеш:
```ruby
hh = {11 => 222, 22 => 333}
hh.clear # очистка хеша
```

#### Удалить элемент из хеша:

```ruby
hh = {'123' => 333, '444' => 7594}
hh.delete '123'
```

#### Решение задачи:

```ruby
# хеш для хранения данных
@hh = {}

# добавление пары в хеш
def add_person name, age
  
  # if @hh[name]
	#	 puts "Такой пользователь уже существует!"
	# end
  
  puts "Такой пользователь уже существует!" if @hh[name]
  
	@hh[name] = age
end

# отображение содержимого хеш
def show_hash
	@hh.each do |name, age|
		puts "#{name} is #{age} years old."
	end
end

while true
	# добавлять пока не введена пустая строка
	print "Enter name: "
	@name = gets.strip.capitalize

	if @name == ""
		show_hash
		exit
	end

	print "Enter age: "
	@age = gets.to_i

	add_person @name, @age
end
```

#### If в одну строку:

```ruby
a = 10
b = 20

puts 'OK' if a+b == 30
```
Слева можно писать любое выражение:
```ruby
hh['Mike'] = 65 if a+b == 30
```

#### Перенос параметров метода в хеш:

```ruby
def add_person options
  name = options[:name]
  age = options[:age]
end

options = {:name => 'Mike', :age => 44}
```

```ruby
add_person :name => "Mike", :age => 22
```

> Хеш в качестве параметра метода можно использовать только в конце перечисленных параметров

```ruby
def say_hello param1, param2, hash_param
  # code
end
```


### Merge (hash):

Объединение хешей

```ruby
# выводит на экран записную книгу
def show_book book
	book.each do |name, age|
		puts "#{name} is #{age} years old"
	end
end

book1 = { 'Mike' => 65, 'Joe' => 12 }
book2 = { "John" => 88, 'Alisha' => 44 }

show_book book1
show_book book2

# or you can merge 2 hashes
book = book1.merge book2
show_book book

#or you can merge 2 hashes with .merge!
book1.merge! book2
show_book book1
```
### Перенаправление вывода:

```ruby
ruby app.rb > file.txt
ruby app.rb >> file.txt
```
знак > заменяет файл (ВНИМАНИЕ!)
знак >> добавляет в конец файла

> вносим в нашу программу html-теги и затем делаем вывод в файл app.rb > file.html - получился вывод в html

Следующий урок: https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md