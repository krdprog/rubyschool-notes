## Урок 29 - введение в Active Record

#### Gemfile

```ruby
source "https://rubygems.org"

gem "sinatra"
gem "sinatra-contrib"
gem "sqlite3"
gem "activerecord"
gem "sinatra-activerecord"

group :development do
  gem "tux"
end
```
#### Установка
```bash
bundle install
```
gem tux

deploy - развёртывание

#### Подключение базы и ActiveRecord
```ruby
require 'sinatra'
require 'sinatra/reloader'
require 'sinatra/activerecord'

set :database, "sqlite3.my_database.db"

get '/' do
  erb :index
end
```

Создадим сущности

```ruby
class Client < ActiveRecord::Base
end
```

В терминале запустить гем tux
```bash
tux
```
Список всех сущностей в БД (набрать в tux):
```ruby
Client.all
```

### rake

#### Создать Rakefile
```bash
touch Rakefile
```

Содержимое Rakefile:
```ruby
require "./app"
require "sinatra/activerecord/rake"
```

> У меня в Debian не заработало, выпадали ошибки. Удалил ruby и поставил всё через rvm - https://rvm.io/

### Install RVM:
```ruby
sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable
source ~/.rvm/scripts/rvm
rvm install 2.5.3
rvm use 2.5.3 --default
ruby -v
```
> LINK
> command line ruby cheat sheets: http://cheat.errtheblog.com/s/rvm

Всё заработало, идём дальше...

#### Список параметров:
```ruby
rake -T
```
NOTE: Работает только при наличии Rakefile:
```ruby
# Rakefile

require "./app"
require "sinatra/activerecord/rake"
```
Rakefile произошёл от сишного Makefile

#### 3 команды rake:

создаёт новую миграцию в db/migrate/:
```ruby
rake db:create_migration NAME=name_of_migration
```
применяет (выполняет) созданную миграцию:
```ruby
rake db:migrate
```
возврат к предыдущей миграции:
```ruby
rake db:rollback
```

#### Все команды rake:
```ruby
rake db:create              # Creates the database from DATABASE_URL or con...
rake db:create_migration    # Create a migration (parameters: NAME, VERSION)
rake db:drop                # Drops the database from DATABASE_URL or confi...
rake db:environment:set     # Set the environment value for the database
rake db:fixtures:load       # Loads fixtures into the current environment's...
rake db:migrate             # Migrate the database (options: VERSION=x, VER...
rake db:migrate:status      # Display status of migrations
rake db:rollback            # Rolls the schema back to the previous version...
rake db:schema:cache:clear  # Clears a db/schema_cache.yml file
rake db:schema:cache:dump   # Creates a db/schema_cache.yml file
rake db:schema:dump         # Creates a db/schema.rb file that is portable ...
rake db:schema:load         # Loads a schema.rb file into the database
rake db:seed                # Loads the seed data from db/seeds.rb
rake db:setup               # Creates the database, loads the schema, and i...
rake db:structure:dump      # Dumps the database structure to db/structure.sql
rake db:structure:load      # Recreates the databases from the structure.sq...
rake db:version             # Retrieves the current schema version number

```
#### Миграция - это очередная версия нашей базы данных.

Создадим миграцию:

```bash
rake db:create_migration NAME=create_clients
```
Создаётся каталог db/migrate и миграция.

Открываем созданный файл и создаём таблицу в базе данных:

```ruby
# File db/migrate/389173729_create_clients.rb

class CreateClients < ActiveRecord::Migration[5.2]
  def change
    create_table :clients do |t|
      t.text :name
      t.text :phone
      t.text :datestamp
      t.text :barber

        t.timestamps
    end
  end
end

```
> id primary key будет создан автоматически

> t.timestamps создаст 2 дополнительных столбца created_at и updated_at
дата создания и обновления сущности

text -> TEXT

string -> VARCHAR(255)

#### Запустить миграцию:
```bash
rake db:migrate
```
Мы настроили mapping (ORM - Object Relational Mapper)

Связка ООП с реляционными БД

Добавим таблицу barbers:
```ruby
# + in app.rb

class Barber < ActiveRecord::Base
end
```
Создаём миграцию (bash):
```bash
rake db:create_migration NAME=create_barbers
```
Правим миграцию (создаём таблицу и вносим данные):
```ruby
# db/migrate/37837298_create_barbers.rb

class CreateBarbers < ActiveRecord::Migration[5.2]
  def change
    create_table :barbers do |t|
      t.text :name

      t.timestamps
    end

    Barber.create :name => "Joe Doe"
    Barber.create :name => "Elon Musk"
    Barber.create :name => "Alisha Moon"
    Barber.create :name => "Marie Fooo-bar"

  end
end
```
Запустим миграцию:
```bash
rake db:migrate
```

> Совет от @krdprog:
> чтобы в командной строке sqlite3 каждый раз не писать, показывать в столбец и с заголовками, создайте в домашней директории файл .sqliterc с содержимым:
```bash
.headers on
.mode column
```
Теперь, это настройки по-умолчанию.

Едем дальше...

Откроем консоль tux
```bash
tux
```
И введём:
```bash
Barber.count
```
Из этой консоли можно создать дополнительные сущности.

#### Замечание:
```ruby
Barber.create # создаёт в БД

b = Barber.new # создаёт в памяти
b.save # после этого надо сделать
```
Создадим в консоли tux нового парикмахера:
```bash
Barber.create :name => 'Faz Maz'
```
или через .new
```bash
b = Barber.new :name => 'Vasya' # создадим, но не сохраним в базе
b.new_record? # покажет, новый ли это объект
b.save # сохраним в базе
```
Все записи Barber:
```bash
Barber.all
```
rake db:migrate надо запускать в каталоге приложения, где Rakefile

> **Изучи ссылку:**
> Active Record Query Interface — Ruby on Rails Guides; https://guides.rubyonrails.org/active_record_querying.html

Создадим вывод на страницу список наших парикмахеров:
```ruby
# + in app.rb

get '/' do
  @barbers = Barber.all
  erb :index
end
```

```ruby
# + in views/index.erb

<h2>Список парикмахеров:</h2>

<ul>
<% @barbers.each do |barber| %>
  <li><%= barber.name %></li>
<% end %>
</ul>
```
Сортировка .order:
```ruby
@barbers = Barber.order "created_at DESC"
```

Домашнее задание:
1. сделать сохранение записи к парикмахеру в БД с помощью ActiveRecord
2. сделать сущность Contact и на странице /contacts сохранять в БД данные с помощью ActiveRecord

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md