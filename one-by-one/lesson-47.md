## Урок 47

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

### Capybara

> https://www.rubydoc.info/github/teamcapybara/capybara/master

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

new_contacts_path - это именованный маршрут для /contacts

**Создадим каталог /spec/features** и создадим файл /spec/features/visitor_creates_contact_spec.rb:

```ruby
require "rails_helper"

feature "Contact creation" do
  scenario "allows acess to contacts page" do
    visit new_contacts_path

    expect(page).to have_content 'Contact us'
  end
end
```

### Работа с i18n (internationalization)

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

**Применение i18n в Capybara:** Исправим наш тест с учётом i18n файл /spec/features/visitor_creates_contact_spec.rb:

```ruby
require "rails_helper"

feature "Contact creation" do
  scenario "allows acess to contacts page" do
    visit new_contacts_path

    expect(page).to have_content I18n.t('contacts.contact_us')
  end
end
```

Запустим тест:

```bash
rake spec
```

#### Следующий тест, создание самого сообщения:

Откроем страницу с формой заявки и откроем код формы, чтобы узнать id (будем использовать в тесте):

```html
<input name="contact[email]" id="contact_email" type="text">
<textarea name="contact[message]" id="contact_message"></textarea>
```

Наш файл с тестом (features/visitor_creates_contact_spec.rb) будет выглядеть так:

```ruby
require "rails_helper"

feature "Contact creation" do
  scenario "allows acess to contacts page" do
    visit new_contacts_path

    expect(page).to have_content I18n.t('contacts.contact_us')
  end

  scenario "allows a guest to create contact" do
    visit new_contacts_path
    fill_in :contact_email, with: 'foo@bar.ru'
    fill_in :contact_message, with: 'Foo Bar Baz'

    click_button 'Send message'
    expect(page).to have_content 'Thanks!'
  end

end
```

#### Следующий тест: протестировать функциональность приложения залогинившись под пользователем

Сделаем сначала **тест для гостя, что он может зарегистрироваться на сайте, т.е. протестируем форму регистрации.**

Создадим файл /spec/features/visitor_creates_account_spec.rb

```ruby
require "rails_helper"

feature "Account Creation" do
  scenario "allows guest to create account" do
    visit new_user_registration_path

    fill_in :user_username, with: 'FooBar'
    fill_in :user_email, with: 'foo@bar.com'
    fill_in :user_password, with: '1234567'
    fill_in :user_password_confirmation, with: '1234567'

    click_button 'Sign up'
    expect(page).to have_content I18n.t('devise.registrations.signed_up')
  end
end
```

devise.registrations.signed_up взято из i18n - config/locales/devise.en.yml

Запустим тест:

```bash
rake spec
```

Всё это работает с базой данных test.sqlite3

**Далее проверим функциональность создания статей зарегистрированным пользователем:**

__на оформлении__


> Изучить документацию Capybara:
> - https://github.com/teamcapybara/capybara
> - https://www.rubydoc.info/github/teamcapybara/capybara/master

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md