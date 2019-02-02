## Урок 13

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
  #  puts "Такой пользователь уже существует!"
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

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |