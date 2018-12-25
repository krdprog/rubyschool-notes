### Урок 38

#### Разбор вопросов на интервью:

#### Задача: перевернуть все слова в предложении:

```ruby
string = "В лесу родилась ёлочка в лесу она росла"

puts string.split(/ /).reverse.join(' ')
```
На собеседовани при постановке тестовой задачи всегда уточнять требования.


#### Продолжаем разбор Rails. Создадим /contacts в нашем блоге:

http://localhost:3000/contacts

Routing Error. No route matches [GET] "/contacts"

```bash
rails g controller contacts
```
Нам нужно:
- получение страницы с сервера (форма с текстовыми полями) - get (new)
- отправка данных - post (create)

Мы хотим добавить только new и create, поэтому добавим:
```ruby
resource :contacts, only: [:new, :create]
```

Добавим в /config/routes.rb

```ruby
Rails.application.routes.draw do
  get 'home/index'

  resource :contacts, only: [:new, :create]
  resources :articles
end
```
Создадим представление: /app/views/contacts/new.html.erb

Откроем http://localhost:3000/contacts/new

Далее создадим модель, определимся с формой, полями:

```bash
rails g model Contact email:string message:text
```
ActiveRecord создал миграцию и файл модели + юнит-тест.

- /db/migrate/20181210205054_create_contacts.rb
- /app/models/contact.rb

```bash
rake db:migrate
```
У Ruby on Rails есть консоль. Запустим её:

```bash
rails console
```
и введём в неё:
```txt
Contact.all
```
Чтобы узнать какие свойства (поля) у сущности (модели) введём:

```txt
Contact.attribute_names
```
#### Продолжим делать блог:

Добавим в /app/views/contacts/new.html.erb:
```ruby
<h2>Contact us</h2>

<%= form_for :contact, url: contacts_path do |f| %>
<p>
  <%= f.label :email %>
  <%= f.text_field :email %>
</p>
<p>
  <%= f.label :message %>
  <%= f.text_area :message %>
</p>

<p>
  <%= f.submit 'Send message' %>
</p>

<% end %>
```
Откроем http://localhost:3000/contacts/new

- :new получает GET
- :create получает POST

> Изучи ссылку: Rails Routing from the Outside In — Ruby on Rails Guides
> https://guides.rubyonrails.org/routing.html

> Изучи ссылку: Rusrails: Роутинг в Rails
> http://rusrails.ru/rails-routing

> Rails Routing from the Outside In — Ruby on Rails Guides
> https://guides.rubyonrails.org/routing.html#singular-resources

Поменять название кнопки отправки формы можно так:
```ruby
<%= f.submit 'Send message' %>
```
> Для contact используем в единственном числе. Если у нас эта сущность будет одна и не надо выводить /contacts/1 .. /contacts/2 ..., то используем resource. Если много сущностей, используем resources.

Добавим предствление /app/views/contacts/create.html.erb

Запишем отправленные данные через форму в базу данных.

Откроем /app/controllers/contacts_controller.rb и запишем код, но...

```ruby
class ContactsController < ApplicationController

  def new
  end

  def create
    @contact = Contact.new(params[:contact])
    @contact.save
  end

end
```
При обращении к этим параметрам будет выпадать ошибка, т.к. аттрибуты запрещены (связано с безопасностью). Нам надо их разрешить, для этого используется специальный синтаксис:

```ruby
class ContactsController < ApplicationController

  def new
  end

  def create
    @contact = Contact.new(contact_params)
    @contact.save
  end

  private

  def contact_params
    params.require(:contact).permit(:email, :message)
  end

end
```
Если мы добавим ещё параметр, то и его надо будет указать в params.require(:contact).permit(:email, :message)

Теперь мы можем добавить запись в БД через форму http://localhost:3000/contacts/new

Проверим в rails console:
```txt
Contact.all
```

#### Сделаем валидацию нашей модели. Модели находятся в каталоге /app/models/

Валидацию надо добавить в /app/models/contact.rb

```ruby
class Contact < ApplicationRecord
  validates :email, presence: true
  validates :message, presence: true
end
```
и исправим код в контроллере /app/controllers/contacts_controller.rb:

```ruby
def create
  @contact = Contact.new(contact_params)
  if @contact.valid?
    @contact.save
  else
    render action: 'new'
  end
end
```
Если при заполнении формы будет ошибка валидаци, то форм билдер нам подскажет, обернув неправильно заполненное поле в div:
```html
<div class="field_with_errors"></div>
```
Оформление можно оформить через CSS.

Добавим стиль в файл /app/assets/stylesheets/application.css

```css
.field_with_errors input,
.field_with_errors textarea {
  border: 3px solid red;
}
```
Есть и другие форм-билдеры, мы использовали стандартный form_for, есть ещё Simple form. Для этого подключается отдельный гем.

Сейчас у нашего блога при заходе на http://localhost:3000/contacts выпадает Routing Error. No route matches [GET] "/contacts"

Сделаем так, чтобы по этому адресу у нас открывалась форма из contacts/new

Есть 2 способа.

1. Откроем /config/routes.rb и добавим код:
```ruby
  get 'contacts' => 'contacts#new'
```
Обратить внимание, что при этом мы удалили :new из строки:
```ruby
  resource :contacts, only: [:create]
```
Весь файл:
```ruby
Rails.application.routes.draw do
  get 'home/index'

  get 'contacts' => 'contacts#new'
  resource :contacts, only: [:create]
  resources :articles
end
```
2. Альтернативный способ при котором 1 строка отвечает за 1 сущность:

```ruby
Rails.application.routes.draw do
  get 'home/index'

  resource :contacts, only: [:new, :create], path_names: { :new => '' }
  resources :articles
end
```
Перенаправление по базовому пути: path_names: { :new => '' }

#### Сделаем вывод списка ошибок:

Откроем /app/views/contacts/new.html.erb и добавим код:

```ruby
<p><%= @contact.errors.full_messages %></p>
```
В app/controllers/contacts_controller.rb можно определить переменную:

```ruby
def new
  @contact = Contact.new
end
```
Либо записать вот так (в /app/views/contacts/new.html.erb):

```ruby
<% if @contact && @contact.errors.any? %>
  <p><%= @contact.errors.full_messages %></p>
<% end %>
```
Тогда этот блок при отсутствии переменной (т.е. при /new) не будет отображаться на странице.

Оформим вывод ошибок в виде списка:

```ruby
<% if @contact && @contact.errors.any? %>
  <ul>
    <% @contact.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
  </ul>
<% end %>
```

#### Сравним 2 таблицы

> https://guides.rubyonrails.org/routing.html#singular-resources и https://guides.rubyonrails.org/routing.html#crud-verbs-and-actions


(resource) singular-resources:

```txt
HTTP Verb     Path              Controller#Action   Used for
GET           /geocoder/new     geocoders#new       return an HTML form for creating the geocoder
POST          /geocoder         geocoders#create    create the new geocoder
GET           /geocoder         geocoders#show      display the one and only geocoder resource
GET           /geocoder/edit    geocoders#edit      return an HTML form for editing the geocoder
PATCH/PUT     /geocoder         geocoders#update    update the one and only geocoder resource
DELETE        /geocoder         geocoders#destroy   delete the geocoder resource
```
(resources) crud-verbs-and-actions:

```txt
HTTP Verb     Path            Controller#Action   Used for
GET           /photos         photos#index        display a list of all photos
GET           /photos/new     photos#new          return an HTML form for creating a new photo
POST          /photos         photos#create       create a new photo
GET           /photos/:id     photos#show         display a specific photo
GET           /photos/:id/edit  photos#edit       return an HTML form for editing a photo
PATCH/PUT     /photos/:id     photos#update       update a specific photo
DELETE        /photos/:id     photos#destroy      delete a specific photo
```

- В resources есть метод выводящий всех ресурсов.
- Следующее отличие - это /show

Профиль у пользователя 1 - resource

#### Домашнее задание:

1. FizzBuzz тест: вывести список чисел от 1 до 100, если число делится на 3 писать Fizz, если число делится на 5 писать Buzz, если и на 3 и на 5 писать FizzBuzz иначе выводить просто само число.

2. Создать в нашем блоге страницы:
- /terms - условия использования
- /about - о нашем блоге

3. Реализовать в блоге articles#create

4. Сделать редактирование статьи.

5. Сделать вывод списка всех статей.

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md