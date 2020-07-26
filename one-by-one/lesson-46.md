## Урок 46

#### Тестирование моделей

Добавить в Gemfile нашего RailsBlog:

```ruby
group :test, :development do
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

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |