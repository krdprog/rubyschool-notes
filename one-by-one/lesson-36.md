### Урок 36

HTTP запросы:

- GET /cart?...&...
- POST
- PUT
- DELETE

GET, POST можно отправлять с помощью form

PUT, DELETE через POST с добавлением скрытой переменной.

#### Паттерн REST - передача состояния объекта

В REST существует 7 различных методов с помощью которых можно управлять объектами:

- index
- show
- new
- create
- edit
- update
- destroy

REST product:

- index - /products - все наши продукты - get
- show - /products/1 - конкретный продукт - get
- new - /products/new - вывод формы для создания - get
- create - /products - создание продукта, обращ. к params и в БД создаёт запись - post
- edit - /products/1/edit - возвращает форму для редактирования определённого продукта - get
- update - /products/1 - put
- destroy - /produts/1 - delete

> [Task Them All - Painless to-do lists to organize your [remote] workers](http://web.archive.org/web/20140929220047/http://taskthemall.com:80/)

#### Продолжаем PizzaShop:

Очистим корзину после того как заказ был отправлен и добавим кнопку очистки корзины.

```ruby
# + post '/cart'

# если корзина пустая
if @items.length == 0
  return erb "В корзине нет товаров"
end
```

#### Минусы созданного PizzaShop:
- модели из app.rb надо вынести в отдельный каталог
- много несвязанных между собой get, post в одном файле
- вспомогательный метод (хелпер) в этом же app.rb
- представления не в подкаталогах (как в рейлс)
- бардак с url
- в js дублирование кода
- нет тестов (в рейлс для всего существуют тесты)

Далее будем писать на Rails и постепенно эти минусы будут уходить.