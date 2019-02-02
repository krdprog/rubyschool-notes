## Урок 43

Заменим жёстко прописанные ссылки, на ссылки с именованными маршрутами:

```ruby
<p><%= link_to "Sign In", new_user_session_path %> | <%= link_to "Sign Out", destroy_user_session_path, method: :delete %></p>
```

#### Далее, выведем имя пользователя при авторизации, и уберём Sign Out ссылку, когда мы не авторизованы.

> https://github.com/plataformatec/devise#controller-filters-and-helpers

```ruby
<% if user_signed_in? %>
  Hello, <%= current_user.email %> | <%= link_to "Sign Out", destroy_user_session_path, method: :delete %>
<% else %>
  <%= link_to "Sign In", new_user_session_path %>
<% end %>
```

#### Авторизация, сессии

- Аутентификация - проверка пользователя и пароля
- Авторизация - наделение определёнными правами в зависимости от роли (юзер/админ)

```txt
       Login, password
User ------------------> Server
     <------------------
       Cookies (*)
```

/config/initializers/devise.rb

```ruby
# config.secret_key =
```

#### Механизм сессий

```ruby
session['key'] = 'value'
```

Cookie выдаётся при первом обращении к серверу, и затем без авторизации идёт обмен данными.

#### JSON - универсальный формат данных

> https://ru.wikipedia.org/wiki/JSON

Следующий пример показывает JSON-представление объекта, описывающего человека. В объекте есть строковые поля имени и фамилии, объект, описывающий адрес, и массив, содержащий список телефонов. Как видно из примера, значение может представлять собой вложенную структуру.

```json
{
   "firstName": "Иван",
   "lastName": "Иванов",
   "address": {
       "streetAddress": "Московское ш., 101, кв.101",
       "city": "Ленинград",
       "postalCode": "101101"
   },
   "phoneNumbers": [
       "812 123-1234",
       "916 123-4567"
   ]
}
```

#### Добавление username в наш блог, и вставка вместо поля автора комментария, username залогиненого пользователя:

```bash
rails g migration add_username
```

Добавим в существующую таблицу новый столбец.

> https://api.rubyonrails.org/classes/ActiveRecord/Migration.html

/db/migrate/20190129063426_add_username.rb

```ruby
class AddUsername < ActiveRecord::Migration[5.2]
  def change
    add_column :users, :username, :string
    add_index :users, :username, unique: true
  end
end
```

```bash
rake db:migrate
```

#### Индекс add_index:

Индекс - увеличивается время вставки, но уменьшается время выборки по определённому полю.

#### Devise надо указать какие дополнительные параметры можно задать.

Все контроллеры наследуются от базового контроллера ApplicationController и чтобы задать для всех контроллеров один параметр, надо прописать это в ApplicationController.

before_action (в ранних версиях Rails это было before_filter), фильтрует методы в контроллерах до того как их обработать.

/app/controllers/application_controller.rb:

```ruby
class ApplicationController < ActionController::Base
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username])
  end
end
```

#### Далее, отредактируем форму регистрации Sign Up:

Сгенерируем набор views:

```bash
rails g devise:views
```

Добавим в /app/views/devise/registrations/new.html.erb код поля ввода username:

```ruby
<div class="field">
  <%= f.label :username %><br />
  <%= f.text_field :username, autocomplete: "username" %>
</div>
```

#### И, выведем имя вместо e-mail в /app/views/layouts/application.html.erb:

```ruby
<% if user_signed_in? %>
  Hello, <%= current_user.username %> | <%= link_to "Sign Out", destroy_user_session_path, method: :delete %>
<% else %>
  <%= link_to "Sign In", new_user_session_path %>
<% end %>
```

#### Сделаем так, чтобы при комментировании статьи автор указывался тот, кто залогинен

Авторизация только для создания и редактирования статьи /app/controllers/articles_controller.rb:

```ruby
before_action :authenticate_user!, :only => [:new, :create, :edit, :update, :destroy]
```

И, для комментирования /app/controllers/comments_controller.rb:

```ruby
before_action :authenticate_user!, :only => [:create]
```

**Домашнее задание:**

- Найти и изучить, что такое индексы в БД.
- Сделать так, чтобы комментарии оставлялись под именем залогиненого пользователя.

> https://andreyex.ru/bazy-dannyx/uchebnoe-posobie-po-sql/sql-indeksy/

> http://www.sql.ru/articles/mssql/03013101indexes.shtml

> https://habr.com/ru/post/247373/

> https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D0%B4%D0%B5%D0%BA%D1%81_(%D0%B1%D0%B0%D0%B7%D1%8B_%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85)

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |