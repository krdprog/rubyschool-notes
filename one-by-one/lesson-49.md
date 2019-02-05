## Урок 49

### Про паттерны программирования. Полиморфные ассоциации:

Создадим приложение poly_demo

```bash
rails new poly_demo
```

При написании приложения у нас могут быть сущности, которые содержит одинаковое поведение, одинаковые свойства и нам захочется использовать DRY-принцип, и в этом нам поможет следующее:

```text
Post - PostComment (x)

Image - ImageComment (x)

Link - LinkComment (x)

Article - ArticleComment (x)
```

Нам нет смысла создавать отдельные сущности для подвидов комментариев.


```text
Post
          \
Image    --  Comment
          /
Link

Article


Post.comments
Image.comments
Link.comments
Article.comments


content
rating
```

Одна модель может принадлежать разным сущностям, но при этом оставаться сама собой (полиморфизм).

```bash
rails g model Comment content:text
```

```bash
rails g model Post content:text
```

```bash
rails g model Image url:text
```

#### Свяжем эти сущности.

Т.к. нам надо связать сущность комментарий с несколькими сущностями, belongs_to делаетс немного иначе, чем обычно.

При связывании с полиморфной ассоциацией надо в belongs_to добавить с окончанием **able**

/app/models/comment.rb:
```ruby
class Comment < ApplicationRecord
  belongs_to :commentable, polymorphic: true
end
```
commentable - получается: комментируемый

Хендл, рукоятка, которая существует у других сущностей.

/app/models/post.rb:
```ruby
class Post < ApplicationRecord
  has_many :comments, as: :commentable
end
```

/app/models/image.rb:
```ruby
class Image < ApplicationRecord
  has_many :comments, as: :commentable
end
```

Далее, откроем миграцию db/migrate/20190205095251_create_comments.rb и добавим строку:

```ruby
t.references :commentable, polymorphic: true
```

```ruby
class CreateComments < ActiveRecord::Migration[5.2]
  def change
    create_table :comments do |t|
      t.text :content
      t.references :commentable, polymorphic: true
      t.timestamps
    end
  end
end
```

Далее, делаем:

```bash
bundle exec rake db:migrate
```

Далее, откроем Rails-консоль:

```bash
rails console
```

```ruby
post = Post.create(content: 'Foo bar')
post.comments
post.comments.create(content: 'Baz Buuu Foo')
post.comments.create(content: 'Comment 2')
post.comments
image = Image.create(url: '1.jpg')
image.comments.create(content: 'Wow! Super!')
image.comments.create(content: 'This is comment for image!')
image.comments
image2 = Image.create(url: '2.jpg')
image2.comments.create(content: 'Bar')
```

Посмотрим базу данных /db/development.sqlite3:

```bash
sqlite3 development.sqlite3
```

```bash
select * from comments;
select * from posts;
select * from images;
```

### Про развёртывание на Heroku

> См. инструкцию тут: https://github.com/krdprog/prog-conspects/blob/master/hartl-rails.md

### Паттерн Singleton (один объект на всех)

Банда Четырёх - GoF (Gang of Four)

> https://habr.com/ru/post/210288/

> https://ru.wikipedia.org/wiki/Design_Patterns

#### Hапишем логгер, который будет сначала выводить на экран, а затем сохранять в файл.

app.rb:
```ruby
class Logger
  def say_foo
    puts "Fooo!"
  end
end

logger = Logger.new
logger.say_foo
```
Создадим ещё логгеры. Есть недостаток такого подхода. В этом случае, объектов создаётся много, а действие совершается одно.

Попробуем исправить:

app.rb:
```ruby
class Logger
  def self.say_foo
    puts "Fooo!"
  end
end

Logger.say_foo
```

Добавим ещё метод:

```ruby
class Logger
  def self.say_foo
    puts "Fooo!"
  end

  def self.log_foo bar
    f = File.open 'log.txt', 'a'
    f.puts bar
    f.close
  end
end

Logger.say_foo
Logger.log_foo 'Wow!'
Logger.log_foo 'Meow!'
```

Недостаток в том, что мы постоянно открываем и закрываем файл log.txt - это увеличивает нагрузку.

class method
```ruby
Logger.say_foo)
```
instance method
```ruby
logger = Logger.new
logger.say_foo
```

**Чтобы решить это, нам потребуется паттерн Singleton**

Наша цель сделать один объект на всех.


Всё это принадлежит к экземпляру класса (к объекту):

@foo - instance variables

instance method:
```ruby
def foo
end
```

Это принадлежит к статическому классу:

@@foo - class variable

class method:
```ruby
def self.foo
end
```

**Решение:**

logger.rb:
```ruby
class Loggeer
  def initialize
    @f = File.open 'log.txt', 'a'
  end

  # сделаем, чтобы метод возвращал экземпляр нашего класса
  @@x = Loggeer.new

  def self.instance
    return @@x
  end

  # instance method
  def log bar
    @f.puts bar
    @f.flush
  end

  # механизм защиты, чтобы Loggeer.new можно было написать только внутри класса
  private_class_method :new
end
```

app.rb:
```ruby
require "./logger"

Loggeer.instance.log "Good job!"
```

**В стандартной библиотеке ruby есть модуль Singleton**

> https://ruby-doc.org/stdlib-2.5.3/libdoc/singleton/rdoc/Singleton.html

Перепишем программу используя этот встроенный модуль.

logger.rb:
```ruby
require 'singleton'

class Loggeer

  include Singleton

  def initialize
    @f = File.open 'log.txt', 'a'
  end

  def log bar
    @f.puts bar
    @f.flush
  end

end
```
app.rb:
```ruby
require "./logger"

Loggeer.instance.log "It`s work!"
```

> https://github.com/krdprog/logger.rb

**Домашнее задание:** сделать блог с сущностями post, link, image с комменатриями, сделать на главной странице вывод всех этих сущностей.

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |