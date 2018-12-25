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