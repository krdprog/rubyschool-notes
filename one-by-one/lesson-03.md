## Урок 3

```bash
mkdir -p dir/subdir/subdir2/foo/bar
```

резервное копирование проектов рекомендует в каталог bak/1 ... n, но используй лучше git


В Ruby любой метод обязательно возвращает значение. Если передать результат метода `puts` в переменную то он вернет `nil`.

```console
irb(main):001:0> value = puts "Ruby"
Ruby
irb(main):002:0> value
=> nil
```

Метод puts всегда возвращает значение `nil`.

_В Ruby всё - объект_

Метод `class`:

```ruby
1.class
"foobar".class
1.23.class
```
вернёт тип данных объекта.

Основные типы данных в Ruby:

```ruby
string - "2"
fixnum - 2
float - 2.1
array - [...]
hash - {...}
bignum - 2^30+1
```

Начиная с версии 2.4 Ruby объединяет классы `Fixnum` и `Bignum` в `Integer`.

- https://ru.wikibooks.org/wiki/Ruby
- https://ru.wikibooks.org/wiki/Ruby/Базовые_типы_данных

Преобразование типов данных:
  - `.to_i` - в число
  - `.to_f` - в число с плавающей точкой
  - `.to_s` - в строку

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |