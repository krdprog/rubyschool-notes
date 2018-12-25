### Урок 21

> Посмотри ссылку про git: http://think-like-a-git.net/

В Sinatra можно создать каталог **public** и всё, что в нём будет размещено, будет доступно из веба.

style.css - надо размещать в каталог /public

> Sinatra Bootstrap: https://github.com/bootstrap-ruby/sinatra-bootstrap

Bash:
```sh
git clone https://github.com/bootstrap-ruby/sinatra-bootstrap.git
mv sinatra-bootstrap/ app
cd app
bundle install
bundle exec ruby app.rb
```
Результат в браузере: http://localhost:4567