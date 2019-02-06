# Конспект RubySchool.us [8]

## Урок 46

#### Тестирование моделей

Добавить в Gemfile нашего RailsBlog:

```ruby
group :test :development do
  gem 'rspec-rails'
  gem 'shoulda-matchers'
  gem 'capybara'
end
```

```bash
bundle install
```

**Настройка Rspec для Rails**

```bash
rails g rspec:install
```

#### Настроим Shoulda-matchers, добавив в spec/rails_helper.rb:

```ruby
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

> Тестирование проводится в разрабатываемом учебном проекте RailsBlog - https://github.com/krdprog/RailsBlog-rubyschool

- Создадутся новые каталоги и хелперы. Создастся каталог /spec
- Создадим для тестирования моделей каталог /spec/models

Модель, которую будем тестировать (/app/models/contact.rb):
```ruby
class Contact < ApplicationRecord
  validates :email, presence: true
  validates :message, presence: true
end
```

Создадим тест для модели: создать файл /spec/models/contact_spec.rb
```ruby
require 'rails_helper'

describe Contact do
  it { should validate_presence_of :email }
  it { should validate_presence_of :message }
end
```

Запустим тест:
```bash
rake spec
```

should - должно

### Shoulda matchers:

> https://relishapp.com/rspec/rspec-expectations/docs/built-in-matchers

> https://matchers.shoulda.io/

> http://matchers.shoulda.io/docs/v3.1.3/

> https://github.com/thoughtbot/shoulda-matchers

#### have_many:

> http://matchers.shoulda.io/docs/v3.1.3/Shoulda/Matchers/ActiveRecord.html#have_many-instance_method

Создадим тест /spec/models/article_spec.rb:

```ruby
require 'rails_helper'

describe Article do
  it { should have_many :comments }
end
```

Создадим тест /spec/models/comment_spec.rb

```ruby
require 'rails_helper'

describe Comment do
  it { should belong_to :article }
end
```

#### Вложенный describe - повышает читаемость тестов

/spec/models/article_spec.rb:

```ruby
require 'rails_helper'

describe Article do
  describe "validations" do
    it { should validate_presence_of :title }
    it { should validate_presence_of :text }
  end

  describe "assotiations" do
    it { should have_many :comments }
  end
end
```

Принято писать для НЕ методов:

```ruby
describe "something" do
```

А, для instance методов:

```ruby
describe "#method" do
```

Для class методов (self.method):

```ruby
describe ".method" do
```

### Добавим в /app/models/article.rb

```ruby
def subject
  title
end
```

И, протестируем этот метод, добавим тест в /spec/models/article_spec.rb

```ruby
require 'rails_helper'

describe Article do
  describe "#subject" do
    it "returns the article title" do
      article = create(:article, title: 'Foo Bar')

      expect(article.subject).to  eq 'Foo Bar'
    end
  end
end
```

Чтобы этот тест заработал, нужен гем factory bot

#### gem Factory Girl (устарел) --> Factory Bot (использовать)

Помогает при тестировании, чтобы не создавать объекты для теста, создаётся фабрика, и она будет создавать нам объекты для теста.

**ВАЖНОЕ ЗАМЕЧАНИЕ!!!** Гем factory girl - устарел, надо использовать factory bot

> https://github.com/thoughtbot/factory_bot/blob/v4.9.0/UPGRADE_FROM_FACTORY_GIRL.md

> https://www.rubydoc.info/gems/factory_bot/file/GETTING_STARTED.md

```text
DEPRECATION WARNING: The factory_girl gem is deprecated. Please upgrade to factory_bot. See https://github.com/thoughtbot/factory_bot/blob/v4.9.0/UPGRADE_FROM_FACTORY_GIRL.md for further instructions.
```

Добавить в Gemfile:
```ruby
group :development, :test do
  gem "factory_bot_rails"
end
```

```bash
bundle install
```

И, добавить конфигурацию в /spec/rails_helper.rb:
```ruby
RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods
end
```

#### Как создавать фабрики:

> https://www.rubydoc.info/gems/factory_bot/file/GETTING_STARTED.md#Defining_factories

**Создадим каталог с фабриками - /spec/factories**

И, создадим файл /spec/factories/articles.rb:

```ruby
FactoryBot.define do
  factory :article do
    title { "Article title" }
    text { "Article text" }
  end
end
```

Преимущество использования factory bot для тестирования в том, что не нужна база данных.

> Как всё выглядит в RailsBlog см. https://github.com/krdprog/RailsBlog-rubyschool

**Создадим в /app/models/article.rb метод last_comment и протестируем его:**

```ruby
def last_comment
  comments.last
end
```

Добавим в /spec/models/article_spec.rb

```ruby
describe Article do
  describe "#last_comment" do
    it "returns the last comment" do

    end
  end
end
```

Создадим фабрику /spec/factories/comments.rb:

```ruby
FactoryBot.define do
  factory :comment do
    author { "Chuck Norris" }
    sequence(:body) { |n| "Comment body #{n}" }
  end
end
```
Sequences (последовательности):

> https://www.rubydoc.info/gems/factory_bot/file/GETTING_STARTED.md#Sequences

См. документацию по create_list (через поиск по странице):

> https://www.rubydoc.info/gems/factory_bot/file/GETTING_STARTED.md

Добавим в /spec/factories/articles.rb:

```ruby
FactoryBot.define do
  factory :article do
    title { "Article title" }
    text { "Article text" }

    # создаём фабрику для создания статьи с несколькими комментариями
    factory :article_with_comments do
      # после создания article
      after :create do |article, evaluator|
        # создаём список из 3-х комментариев
        create_list :comment, 3, article: article
      end
    end
  end
end
```

В /spec/models/article_spec.rb создадим статью с комментариями для тестирования:

```ruby
require 'rails_helper'

describe Article do
  describe "#last_comment" do
    it "returns the last comment" do
      # создаём статью с 3 комментариями
      article = create(:article_with_comments)

      # проверка
      expect(article.last_comment.body).to eq "Comment body 3"
    end
  end
end
```

**Домашнее задание:**

- Добавить в сущность Article валидацию длины title в 140 символов и написать тесты.
- Добавить в сущность Article валидацию длины text в 4000 символов и написать тесты.
- В Comment добавить валидацию длины body в 4000 символов и написать тесты.

## Урок 48

### Скаффолдинг

```bash
rails new toy_app
bundle install
bundle update
```

Сделаем мини-приложение микропостинг:

```text
---------------------
|  users            |
---------------------
id     integer
name   string
email  string
---------------------

---------------------
|  microposts       |
---------------------
id      integer
content text
user_id integer
---------------------
```

```bash
rails g scaffold User name:string email:string
```
Создаст одновременно модель, контроллер, экшены, представления, тесты.

```bash
bundle exec rake db:migrate
```

```bash
rails g scaffold Micropost content:text user_id:integer
```
Валидация Micropost (в модели):

```ruby
validates :content, length: { maximum: 140 }, presence: true
validates :user_id, presence: true
```

Валидация User (в модели):

```ruby
validates :name, presence: true
validates :email, presence: true
```

Добавим ассоциации one-to-many между пользователем и микропостом.

```text
User --1--------*-- Micropost
```

Модель User:

```ruby
has_many :microposts
```

Модель Micropost:

```ruby
belongs_to :user
```

Откроем rails console:

```ruby
first_user = User.first
first_user.microposts
first_user.microposts.count # обратится к БД
first_user.microposts.length # прочитает то, что уже есть в памяти, без БД
micropost = first_user.microposts.first # первый микропост первого пользователя
micropost.user # получить пользователя микропоста
```

- Модели наследуются от ActiveRecord::Base
- Контроллеры наследуются от ApplicationController, а ApplicationController от ActionController::Base

### Отправка e-mail

- SMTP-сервер
- хостинг - большой процент попадания в спам
- gmail - надо включить options, есть ограничения
- postmarkapp - для Transactional emails, но через него нельзя делать почтовую рассылку. Письма должны быть с кнопкой Unsubscribe.
- bulk email messaging - для рассылки

> https://postmarkapp.com/developer/user-guide/sending-email/sending-with-api

> https://github.com/wildbit/postmark-gem

> https://github.com/wildbit/postmark-rails

> http://rusrails.ru/action-mailer-basics

> https://www.youtube.com/watch?v=FNOhpAWbiKA

> https://github.com/mikel/mail

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

## Урок 50

### Регулярные выражения - Regular expression (regex)

> https://rubular.com/

Для отработки текстовых файлов, данных, веб-страниц.

Файл для тренеровки с Regex можно скачать тут: https://github.com/krdprog/rubyschool-notes/blob/master/for-lesson-50-regex.txt

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
Убрать пустые строки:

\n\n заменяем на \n
```

```text

the quick

brown fox



jumps


over

the lazy



dog
```

```text
\n+ заменяем на \n
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

## Содержание конспекта:

|  N    |  N    |  N    |  N    |
|-------|-------|-------|-------|
|  [Урок 01-14](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-01.md)   |  [Урок 15-19](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-02.md)   |  [Урок 20-25](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-03.md)   |  [Урок 26-30](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-04.md)   |
|  [Урок 31-35](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-05.md)   |  [Урок 36-40](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-06.md)   |  [Урок 41-45](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-07.md)   |  [Урок 46-50](https://github.com/krdprog/rubyschool-notes/blob/master/rubyschool-notes-08.md)   |