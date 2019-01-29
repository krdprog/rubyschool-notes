### Урок 43

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

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md