## Урок 10

```ruby
def movie
  action = [:left, :right, :up, :down]

  x = rand(0..3)

  return action[x]
end

def say
  sleep 0.06
end

while true
  speech = movie
  puts "Go to #{speech}"
  sleep 0.4
end
```

capitalize every name:
```ruby
arr = %w{joe matt mary olga oleg peter vasya}

arr.each do |x|
  puts x.capitalize
end
```
индекс arr[2]

```ruby
arr = [1, 2, 3, 4, 5]
arr2 = arr[1..3]

puts arr2 #=> 2 3 4
```
```ruby
arr = %w{aa bb cc dd ee}
arr[1..3][2]
#=> "dd"
```

#### Удаление элемента из массива
```ruby
arr = ["aa", "bb", "cc", "dd"]

arr.delete_at 1 # delete "bb"
arr.delete "dd" # delete "dd"
```

Удаление из массива (задача):
```ruby
person = %w{vasiliy mariya joe bob marley}

while true

  person.size.times do |x|
    puts "#{x}. #{person[x].capitalize}"
  end

  puts ""
  print "Кого удалить? (порядковый номер): "
  del = gets.to_i

  person.delete_at del

  puts "====="

end
```
Или вариант с each

```ruby
person = %w{vasiliy mariya joe bob marley}

while true
  i = 0
  person.each do |x|
    puts "#{i}. #{x.capitalize}"
    i += 1
  end

  puts ""
  print "Кого удалить? (порядковый номер): "
  del = gets.to_i

  person.delete_at del

  puts "====="

end
```

Добавление в массив с выводом на экран:

```ruby
arr = []

while true
  print "Enter name to add: "
  name = gets.strip.capitalize

  if name == ""
    break
  end

  arr << name

  puts "==="

  x = 0

  arr.each do |name|
    puts "#{x}. #{name}"
    x += 1
  end

end
```

### Массивы можно вкладывать в массив.
#### Вывод многомерного массива

```ruby
arr = [[1, 2, 3], [4, 5, 6]]

puts arr[0][1] #=> 2

```

Задание:
```ruby
# enter yor name
# enter your age
# in arr
# print all data

persons = []

while true
  person = []

  print "Enter your name: "
  name = gets.strip.capitalize

  if name == ""
    break
  end

  person << name

  print "Enter your age: "
  age = gets.to_i

  person << age

  persons << person

end

puts "", "===="
puts "RESULT:"

persons.each do |x|
  puts "#{x[0]}, #{x[1]}"
end

puts "====", ""
```
#### Задание: камень, ножницы, бумага:

```ruby
while true

  puts "", "========================"
  print "(R)ock, (S)cissors, (P)aper? "
  s = gets.strip.capitalize

  if s == "R"
    @user_choice = :rock
  elsif s == "S"
    @user_choice = :scissors
  elsif s == "P"
    @user_choice = :paper
  else
    puts "What? I don`t know."
    exit
  end

  arr = [:rock, :scissors, :paper]

  @computer_choice = arr[rand(0..2)]

  # report about win
  def your_win
    puts "You win! Your choice is #{@user_choice} and computer choice is #{@computer_choice}."
  end

  def computer_win
    puts "Computer win! Your choice is #{@user_choice} and computer choice is #{@computer_choice}."
  end

  # game variants
  if @user_choice == @computer_choice
    puts "Nobody wins. Your choice is #{@user_choice} and computer choice is #{@computer_choice}."
  # user:
  elsif @user_choice == :rock && @computer_choice == :scissors
    your_win
  elsif @user_choice == :scissors && @computer_choice == :paper
    your_win
  elsif @user_choice == :paper && @computer_choice == :rock
    your_win
  # computer
  else
    computer_win
  end

end
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |