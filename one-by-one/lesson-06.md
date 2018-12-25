## Урок 6

#### Блоки:

Стиль принятый для однострочника:
```ruby
10.times { puts "Foo!" }
```
Другой синтаксис с несколькими командами:
```ruby
10.times do
  puts "Foo!"
  puts "Bar!"
end
```

```ruby
10.times { |i| puts i }
```
```ruby
10.times do |i|
  puts "Foo!"
end
```
### Счёт не с нуля:
```ruby
# от 10 до 20
10.upto(20) { |i| puts i }

# от 20 до 10
20.downto(10) { |i| puts i }

# OR:
10.upto(20) do |i|
  p i
end

20.downt(10) do |i|
  p i
end
```

Решение задания:
```ruby
# программа делает от 10 до 20 запусков и выводит Foo это количество раз
10.upto(20) do |x|
  1.upto(x) { |i| p "#{i} Foo!" }
end
``` 

Rand Charset:
```ruby
30.times { print rand(70..120).chr }
```
Homework:
```ruby
arr = %w(t e r m i n a t o r)
dash = "-"

arr.size.times do |i|
  print arr[i]
  sleep 0.6
  print dash
  sleep 0.6
end
```

Следующий урок: https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md