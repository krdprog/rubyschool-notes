## Урок 11

#### Поменять значения местами

```ruby
a = 50
b = 20

a, b = b, a

puts a
puts b
```
var 2:
```ruby
a = a + b
b = a - b
a = a - b
```
### Обработка двумерного массива:
```ruby
arr = [ [1, 2], [3, 4], [5, 6] ]

arr.each do |item|
  item.each do |x|
    puts x
  end
end
```

### .each_with_index - метод заменяющий, что делали выше на прошлых уроках:
```ruby
arr = %w{ Vova Masha Petya Alisha }

arr.each_with_index do |item, i|
  puts "#{i} #{item}"
end
```

### Хеши (Hash):

Это структура данных
Key value storage - хранилище ключ значение
В других языках ещё называют Dictionary

```ruby
hash = Hash.new

hash = {}
```

```ruby
hash = { 'Mike' => '3538300',
        'Jessie' => '4348829'}

puts hash['Mike']
```
> => называется Hash Rocket

#### Используй одинарные кавычки, а двойные для интерполяции.

```ruby
a = 2+2

puts "#{a}" #=> 4
puts '#{a}' #=> #{a}
```

```ruby
options = {
  :font_size => 10,
  :font_family => 'Arial',
  :arr => [1, 5, 8]
  }

options[:font_size]
options[:font_family]
options[:arr][2]
```
> для исследования используй .inspect и .class

#### Обращение к хешу по ключу и получаем значение

### Добавление элемента в хеш:

```ruby
hash = {}

hash['Alisha'] = 636400
hash[:foo] = 'Bar'
```
#### Решение задания "Телефонная книга"
```ruby
# Enter name (Enter to stop):
# Enter phone number:
# add to hash

phonebook = {}

while true
  print "Enter name (Enter to stop): "
  name = gets.strip.capitalize

  if name == ""
    break
  end

  print "Enter phone number: "
  number = gets.strip

  phonebook[name] = number
end

puts "", "=== My Phone Book ==="
phonebook.each do |name, phone|
  puts "#{name} number is #{phone}"
end
puts "====================="
```

##### Важное замечание: в хеше через each элементы могут выводиться не по порядку. Хеш не предназначен для хранения элементов по порядку, в отличии от массива.


#### Вызвать нужный элемент из хеша:
```ruby
phonebook['Alisha']
```

#### Решение задачи "Англо-русский переводчик":
```ruby
words = {
  'dog' => 'собака',
  'cat' => 'кошка',
  'frog' => 'лягушка'
  }

while true
  print "Введите слово: "
  user_word = gets.strip.downcase

  if user_word == ""
    break
  end

  puts "Перевод: #{words[user_word]}"
  puts ""
end
```
> У слова может быть несколько переводов, поэтому можно переписать программу, где значение хеша = массив.

```ruby
fruits = []
fruits << {"name"=>"banana", "cost"=>10} << {"name"=>"apple", "cost"=>7}
#=> [{"name"=>"banana", "cost"=>10}, {"name"=>"apple", "cost"=>7}]
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |