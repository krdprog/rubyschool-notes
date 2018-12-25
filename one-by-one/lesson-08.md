## Урок 8

Ruby понимает 10_000_000 как 10 млн.

```ruby
a = 10_000
```
Обрывает работу оператора:
```ruby
break
```
```ruby
"Hello".reverse
```
Ranges:
```ruby
(1..10).each { |x| puts x*4 }
```

```ruby
(1..5).each => 1 2 3 4 5 
(1...5).each => 1 2 3 4

# можно не только цифры:
('aa'..'ff').each { |x| puts x }
('10aa'..'20ff').each { |x| puts x }
```

GUI framework:
[Shoes! The easiest little GUI toolkit, for Ruby.](http://shoesrb.com/)


Следующий урок: https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md