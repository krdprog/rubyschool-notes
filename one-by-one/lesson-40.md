## Урок 40

#### Повторение:

**Чтобы показать обычные статические страницы (типa /terms, /about) нам надо:**

- создать контроллер (rails g controller ...)
- добавить actions (методы terms, about)
- добавить views (terms.html.erb, about.html.erb)
- добавить в routes.rb маршруты (get 'terms' => 'pages#terms')

В синатре это занимало 2 действия:

- маршрут
- views

#### Создание сущности:

- создание модели (rails g model ...)
- rake db:migrate
- добавить маршрут в routes.rb - resource или resources
- добавить контроллер
- в контроллер добавить actions (index, show, new, edit, create, update, destroy)
- добавить views

### Продолжим делать RailsBlog, доделаем удаление Article:

Чтобы удалить сущность, надо сделать 2 действия:

- найти сущность по id
- удалить

Добавим в articles_controller.rb метод destroy:

```ruby
def destroy
  @article = Article.find(params[:id])
  @article.destroy

  redirect_to articles_path
end
```

Добавим в файл /views/articles/index.html.erb кнопку удаления статьи:

```html
<%= link_to "Destroy", article_path(article), method: :delete %>
```

или с подтверждением:

```html
<%= link_to "Destroy", article_path(article), method: :delete, data: { confirm: 'Действительно удалить?'} %>
```

К тегу ссылки на удаление добавится атрибут data-method="delete"

Файл turbolinks обрабатывает атрибуты data-* встречающиеся в коде html

**Разбор data-method="delete"**

- скрипт сканирует страницу
- ищет атрибут data-method="delete"
- создаёт форму, которая будет отправлять с помощью метода delete на URL /articles/2 через POST
- устанавливает свой обработчик, при котором при нажатии на ссылку вызывается сабмит формы

#### Полезная особенность генераторов, возможность указывать reference columns - делать ссылки на другие сущности

```bash
rails g model photo album:references
```

> Изучить ссылку: https://railsguides.net/advanced-rails-model-generators/

#### Добавление комментариев к статьям:

one-to-many:

```text
Article -1------*- Comment
```

Создадим модель Comment:

```bash
rails g model Comment author:string body:text article:references
rake db:migrate
```
Это добавит в файл /models/comment.rb:

```ruby
class Comment < ApplicationRecord
  belongs_to :article
end
```
А, вот содержимое миграции (/db/migrate/12312314_create_comments.rb):

```ruby
class CreateComments < ActiveRecord::Migration[5.2]
  def change
    create_table :comments do |t|
      t.string :author
      t.text :body
      t.references :article, foreign_key: true

      t.timestamps
    end
  end
end
```

Посмотрим в базе данных:

```bash
cd db
sqlite3 development.sqlite3
```

```text
select * from Articles;
.tables
select * from Comments;
```

**Посмотреть через sqlite3, какие поля есть у сущности:**

```text
pragma table_info(articles);
```

Идём дальше, чтобы добавить к статьям комментарии нам надо в /models/article.db добавить:

```ruby
class Article < ApplicationRecord
  has_many :comments
end
```

Таким образом мы связали 2 сущности между собой.

у нас в /config/routes.rb есть строка:

```ruby
resources :articles
```

Допишем и сделаем **вложенный маршрут**:

```ruby
resources :articles do
  resources :comments
end
```

Команда rake routes покажет нам обновлённую карту маршрутов.

**Теперь добавим контроллер:**

```bash
rails g controller Comments
```

**Далее, создадим методы в контроллере.**

Для комментариев нам нужен один метод - create

Посмотрим в rails console:
```text
Comment.all
@article = Article.find(1)
@article.comments.create({ author: 'Foo', body: 'Bar' })
@article.comments
```

Создадим метод create в /app/controllers/comments_controller.rb:

```ruby
class CommentsController < ApplicationController
  def create
    @article = Article.find(params[:article_id])
    @article.comments.create(comment_params)

    redirect_to article_path(@article)
  end

  private

  def comment_params
    params.require(:comment).permit(:author, :body)
  end

end
```

**Добавление формы в представление статьи:**

> Изучить ссылку: https://guides.rubyonrails.org/association_basics.html

> Изучить ссылку: https://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html

```ruby
<p>
<%= form_for([@article, @comment]) do |f| %>
<% end %>
</p>
```

build:
```ruby
<p>
<%= form_for([@article, @article.comments.build]) do |f| %>
<% end %>
</p>
```

**Добавим форму в представление (/app/views/articles/show.html.erb):**

```ruby
<p>
<%= form_for([@article, @article.comments.build]) do |f| %>

  <p>
    <%= f.label :author %>
    <%= f.text_field :author %>
  </p>
  <p>
    <%= f.label :body %>
    <%= f.text_area :body %>
  </p>

  <p><%= f.submit %></p>

<% end %>
</p>
```

Проверим форму, добавив данные. И, посмотрим в рейлс-консоли

```text
Comment.last
Comment.all
@article = Article.find(1)
@article.comments
```

#### Домашнее задание:

- Сделать вывод комментариев на странице статьи
- Избавиться от ненужных маршрутов
- Сравнить код BarberShop на Rails и на Sinatra

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |