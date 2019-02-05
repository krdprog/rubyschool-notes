## Урок 50

### Регулярные выражения - Regular expression (regex)

> https://rubular.com/

Для отработки текстовых файлов, данных, веб-страниц.

Файл для тренеровки с Regex можно скачать тут: https://github.com/krdprog/rubyschool-notes/blob/master/file-for-lesson-50/for-lesson-50-regex.txt

```text
The cat goes catatonic when you put in in catapult
```

```text
cat\b

\b - граница слова

Выберет: cat
```

```text
Yesterday, at 12 AM, he withdrew $600.00 from an ATM. He then spent $200.12 on  groceries. At 3:00 P.M., he logged onto a poker site and played 400 hands of poker. He won $60.41 at first, but ultimately lost a net total of $38.82.

He had only $0.1 in his pocket.
```

```text
\.\d{2}


\. - точка
\d - любая цифра
{2} - количество повторяющихся символов

Выберет: .00 .12 .41 .82

\.\d{1,2}

Выберет: .00 .12 .41 .82 0.1
```

```text
hi world, it's hip to have big thighs
```

```text
hi\b

Выберет: hi
```

```text
this is file.txt

\b заменить на _

Результат: this_is_file.txt
```

**Regex нужны для:**

- проверки
- замены

**В Sublime Text** нажмём Ctrl + H (поиск и замена), и выберем "с Regular Expression"

#### Проблемы на пустом месте

Пробел, табуляция, \n, \r - всё это whitespace

```text
\n\n
\n  \n
```


```text
the quick brown

fox jumps over

the lazy dog
```

```text

```


### Шпаргалка по регулярным выражениям (regex)

```text
[abc]  A single character of: a, b, or c
[^abc]  Any single character except: a, b, or c
[a-z]   Any single character in the range a-z
[a-zA-Z]  Any single character in the range a-z or A-Z
^   Start of line
$   End of line
\A  Start of string
\z  End of string
.   Any single character
\s  Any whitespace character
\S  Any non-whitespace character
\d  Any digit
\D  Any non-digit
\w  Any word character (letter, number, underscore)
\W  Any non-word character
\b  Any word boundary
(...)   Capture everything enclosed
(a|b)   a or b
a?  Zero or one of a
a*  Zero or more of a
a+  One or more of a
a{3}  Exactly 3 of a
a{3,}   3 or more of a
a{3,6}  Between 3 and 6 of a
```

```text
\ - с обратной косой черты начинаются буквенные спецсимволы, а также он используется
если нужно использовать спецсимвол в виде какого-либо знака препинания;
^ - указывает на начало строки;
$ - указывает на конец строки;
* - указывает, что предыдущий символ может повторяться 0 или больше раз;
+ - указывает, что предыдущий символ должен повторится больше один или больше раз;
? - предыдущий символ может встречаться ноль или один раз;
{n} - указывает сколько раз (n) нужно повторить предыдущий символ;
{N,n} - предыдущий символ может повторяться от N до n раз;
. - любой символ кроме перевода строки;
[az] - любой символ, указанный в скобках;
х|у - символ x или символ y;
[^az] - любой символ, кроме тех, что указаны в скобках;
[a-z] - любой символ из указанного диапазона;
[^a-z] - любой символ, которого нет в диапазоне;
\b - обозначает границу слова с пробелом;
\B - обозначает что символ должен быть внутри слова, например, ux совпадет с uxb
или tuxedo, но не совпадет с Linux;
\d - означает, что символ - цифра;
\D - нецифровой символ;
\n - символ перевода строки;
\s - один из символов пробела, пробел, табуляция и так далее;
\S - любой символ кроме пробела;
\t - символ табуляции;
\v - символ вертикальной табуляции;
\w - любой буквенный символ, включая подчеркивание;
\W - любой буквенный символ, кроме подчеркивания;
\uXXX - символ Unicdoe.
```

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |