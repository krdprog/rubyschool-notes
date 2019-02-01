### Урок 47

### Приёмочное тестирование (Acceptance Testing)

Проверка функциональности на соответствие требованиям. Отличие от юнит-тестов, что для этих тестов обычно существует план приёмочных работ. Юнит-тесты - проверка чтобы не сломалось.

> http://protesting.ru/testing/levels/acceptance.html

unit:
```text
describe
  it
```

acceptance:
```text
feature
  scenario
```

feature scenario - это фишка Capybara

feature - особенность
scenario - сценарий

Добавить в Gemfile:

```ruby
group :test do
  gem 'capybara'
end
```

#### 2 типа тестов:

- visitor_..._spec.rb - анонимный пользователь
- user_..._spec.rb - пользователь залогиненый в системе

**Using Capybara with RSpec:**
> https://github.com/teamcapybara/capybara#using-capybara-with-rspec

#### Пример: Для контактной формы существует 2 сценария:

1. Убедиться, что контактная форма существует.
2. Что мы можем эту форму заполнить и отправить

Проведём тестирование контактной формы в учебном приложении RailsBlog.

> https://github.com/krdprog/RailsBlog-rubyschool

Протестируем форму на http://localhost:3000/contacts

**Создадим каталог /spec/features** и создадим файл /spec/features/visitor_create_contact_spec.rb:

```ruby
require "rails_helper"

feature "Contact creation" do
  scenario "allows acess to contacts page" do
    visit '/contacts'

    expect(page).to have_content 'Contact us'
  end
end
```

### Работа с I18n

- Открыть файл /config/locales/en.yml

```yml
en:
  hello: "Hello world"
```

- Обязательно должны быть 2 пробела, иначе yml не заработает.

Настройка в Sublime Text (Preferences - Settings - Syntax Specific):

```text
{
  "tab_size": 2,
  "translate_tabs_to_spaces": true
}
```

Можно создавать перевод для сайта и вызывать константы во views. Например, для русского языка можно создать /config/locales/ru.yml

```text
# To use the locales, use `I18n.t`:
#
#     I18n.t 'hello'
#
# In views, this is aliased to just `t`:
#
#     <%= t('hello') %>
#
# To use a different locale, set it with `I18n.locale`:
#
#     I18n.locale = :es
```

Создадим в /config/locales/en.yml:

```yml
en:
  contacts:
    contact_us: "Contact Us!"
```

И, вызовем в представлении /app/views/contacts/new.html.erb:

```ruby
<h2><%= t('contacts.contact_us') %></h2>
```



---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md