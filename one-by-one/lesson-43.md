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

**Примечание:** в devise начиная с версии 4 параметры Sanitizer Api изменились, используйте вместо этого:

```ruby
devise_parameter_sanitizer.for(:sign_up) << :username
```
это:

```ruby
devise_parameter_sanitizer.permit(:sign_up, keys: [:username])
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md