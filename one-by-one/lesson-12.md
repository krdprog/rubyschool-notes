## Урок 12

#### Перечисление ключей и значений хеша:

```ruby
hash = { right: "Right", left: "Left", top: "Top", down: "Down" }

puts hash.keys
puts hash.values

# ИЛИ
hash.each_key do |key|
  puts key
end

hash.each_value do |value|
  puts value
end

```
hash.keys и hash.values - это массивы, к ним можно применять методы .size и т.п.

#### Задание: посчитать количество переводов

```ruby
words = {
  'dog' => ['собака', 'шавка', 'бобик'],
  'cat' => ['кошка', 'кошечка'],
  'frog' => ['лягушка', 'квакушка'],
  'mouse' => ['мышь', 'мышка']
}

result = 0

words.each_value { |value| result += value.size }

puts "All words: #{result}"

```
#### Проверка наличия ключа в хеше:
```ruby
puts words.has_key? 'cat'
# ИЛИ
puts words.include? 'cat'
```

```ruby
if words.has_key? 'cat'
  puts 'Meowww!'
end

# ИЛИ

if words['cat']
  puts 'Meowww!'
end
```

#### Проверка наличия значения в хеше:

```ruby
puts words.has_value? 'кит'
```
#### Решение "Однорукий бандит" с хешем:

```ruby
# onehand bandit with hash

win_variant = {
  '111' => 100,
  '222' => 200,
  '333' => 300,
  '444' => 400,
  '555' => 500,
  '666' => 600,
  '777' => 7000,
  '888' => 800,
  '999' => 900,
}

money = 100

while true

  puts 'Press ENTER for game...'
  gets
  random = rand(100..999).to_s


  if win_variant[random]
    puts "Win #{win_variant[random]} dollars."
    money += win_variant[random]
  else
    puts "You lost 10 dollars."
    money -= 10
  end


  puts "Combination: #{random}"
  puts "Your balance is #{money}", ""
end
```

### Функции

return

```ruby
def say a
  puts a
end

say "foo"
```
Можно больше параметров задавать, указывать через запятую:
```ruby
def say param1, param2, param3
  # some code
end
```

```ruby
puts "Bar!" if true
puts "Fooo!" if 2+2 = 4
```

#### Защита от неустановленного параметра:

```ruby
def print_details details
  puts details[:name] if details[:name]
  puts details[:age] if details[:age]
  puts details[:address] if details[:address]
end

hh = { name: "Mike", age: 65, address: "123, West Street"}

print_details hh
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md