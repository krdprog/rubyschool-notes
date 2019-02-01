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




---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md