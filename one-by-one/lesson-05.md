## Урок 5

```ruby
5.upto(10) { |x| print x, " " } #=> 5 6 7 8 9 10
50.downto(-50) { |x| print x, " " }
```
i - писать, когда индекс (принимает значение от нуля)


```ruby
.strip # удаляет не только пробелы, но и
# табуляцию и перенос строки вместо чомп
```

```ruby
\n - перенос строки (line feed - lf)
\t - табуляция
\r - возврат на начало строки (carriage return - cr)

cr lf = \r\n
```
вывести обратный слеш:
```ruby
puts "\\" #=> \
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md