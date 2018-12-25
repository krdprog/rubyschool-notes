## Урок 9

### Методы

```ruby
def foo
  
end
```
Методу можно передать какой-либо параметр, и он вернёт какое-либо значение.

```ruby
def gets_password
  print "Введите пароль: "
  return gets.chomp
end

xx = gets_password

puts "Был введён пароль #{xx}"
```

return - можно не писать

По умолчанию руби возвращает результат последнего выражения. 

```ruby
def print_name name
  puts "Hello #{name}"
end

print_name "Alex"
```
Внутри метода все переменые определяются заново (локальные переменные метода)

```ruby
a = 1
def foo
  a = 2
end

puts a # => 1
```
@a - глобальная переменная

### Объекты, экземпляры класса (instance)

"foo"
"foo"
находятся в разных участках памяти, это разные объекты. Чтобы закрепить за одним участком используются _символы_.

#### Символы (symbol):

Обозначается как:
```ruby
:foo
```
Можно переменным назначать значение символов.

true or false:
```ruby
puts "aaa" == "aaa"
puts :aaa == :aaa
puts "aaa".equal? "aaa" # false
puts :aaa.equal? :aaa
```
.equal? - сравнивает объекты по ссылке

#### Проверка id объекта:
```ruby
"aaa".object_id # каждый раз меняется
:aaa.object_id # у символа один и тот же
```
### Массивы (arrays)

[Class: Array (Ruby 2.5.1)](https://ruby-doc.org/core-2.5.1/Array.html)

Массив = набор объектов

```ruby
arr = []

arr2 = %w{foo bar zoo moo faf}

arr << 14
arr << 22

```

Следующий урок: https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md