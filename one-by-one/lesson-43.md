### Урок 43

на оформлении

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