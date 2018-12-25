### Урок 32

#### Разбор вопросов:

> Изучить: 15 Questions to Ask During a Ruby Interview · GitHub
> https://gist.github.com/krdprog/64a463de21fe77a8946019fde6662d67

- классы содержат данные, имеют методы, которые взаимодействуют с этими данными и используются для того, чтобы создавать объекты на основе этих классов
- объект - это экземпляр класса. для некоторых - это коренной класс руби Object
- модуль - механизм, который служит для namespaces (пространства имён) module/end - Namespace::Class.method. Ещё, модули предоставляют механизм для множественного наследования с помощью миксинов. module/end - extend
- три уровня доступа для модулей и классов: public - по умолчанию, protected, private - методы доступные только внутри самого класса

> [ООП с примерами (часть 1) / Хабр](https://habr.com/post/87119/)
> [ООП с примерами (часть 2) / Хабр](https://habr.com/post/87205/)

> [GitHub - ro31337/first-visit-js: Tiny jQuery plugin to display a message to the user on the first visit to a page](https://github.com/ro31337/first-visit-js)

#### localStorage

```js
window.localStorage
```

> HTML5 Web Storage https://www.w3schools.com/html/html5_webstorage.asp

> [Почему не стоит использовать LocalStorage / Хабр](https://habr.com/post/349164/)

> [LocalStorage на пальцах](https://tproger.ru/articles/localstorage/)

> [window.localStorage - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

```js
function foo() {
    var x = window.localStorage.getItem('score');

    window.localStorage.setItem('score', 555);

    alert(x);
}
```

```js
function foo() {
    var x = window.localStorage.getItem('score'); // это как x = hh['score'] в ruby

    // x * 1 - чтобы преобразовать строку в число
    x = x * 1 + 1;

    window.localStorage.setItem('score', x); // hh['score'] = x

    alert(x);
}
```

> Looping through localStorage in HTML5 and JavaScript - Stack Overflow
> https://stackoverflow.com/questions/3138564/looping-through-localstorage-in-html5-and-javascript

#### ActiveRecord

> [ActiveRecord::ConnectionAdapters::TableDefinition](https://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/TableDefinition.html)

> [ActiveRecord::ConnectionAdapters::SchemaStatements](https://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/SchemaStatements.html)

- :primary_key
- :text
- :integer
- :float
- :decimal
- :datetime
- :timestamp
- :time
- :date
- :binary
- :boolean

#### PizzaShop

- цены рекомендуется хранить в базе в минимальных величинах

```ruby
# + to app.rb

require 'sinatra'
require 'sinatra/reloader'
require 'sinatra/activerecord'

set :database, "sqlite3:database.db"

class Product < ActiveRecord::Base
end

get '/' do
    erb :index
end
```
```bash
rake db:create_migration NAME=create_products
```
```ruby
# + to db/migrate/9279387982_create_products.rb

class CreateProducts < ActiveRecord::Migration[5.2]
  def change
    create_table :products do |t|
        t.string :title
        t.text :description
        t.decimal :price
        t.decimal :size
        t.boolean :is_spicy
        t.boolean :is_veg
        t.boolean :is_best_offer
        t.string :path_to_image

        t.timestamps
    end
  end
end
```
seed database - наполнить базу данных

```bash
rake db:create_migration NAME=add_products
```
```ruby
# + to db/migrate/786238472_add_products.rb

class AddProducts < ActiveRecord::Migration[5.2]
  def change
    Product.create :title => 'Гавайская',
        :description => 'Это гавайская пицца',
        :price => 350,
        :size => 30,
        :is_spicy => false,
        :is_veg => false,
        :is_best_offer => false,
        :path_to_image => '/images/01.jpg'

    Product.create :title => 'Пепперони',
        :description => 'Это пицца Пепперони',
        :price => 450,
        :size => 30,
        :is_spicy => false,
        :is_veg => false,
        :is_best_offer => true,
        :path_to_image => '/images/02.jpg'

    Product.create :title => 'Вегетарианская',
        :description => 'Это вегетарианская пицца',
        :price => 400,
        :size => 30,
        :is_spicy => false,
        :is_veg => true,
        :is_best_offer => false,
        :path_to_image => '/images/03.jpg'
  end
end
```

#### Домашнее задание:
- сделать страницу вывода продуктов

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md