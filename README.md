# Лабораторная работа 2

## Как выполнять

-   Сделать форк **вашей** лабораторной работы номер один с названием `up09-lab2-{ваша_фамилия}`

## 1. Создание схемы для приложения posts

1. В файле `posts/models.py` создайте модель Post, которая описывает посты

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=75)
    body = models.TextField()
    slug = models.SlugField()
    date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

> Функция `__str__` позволяет использовать атрибут `title` как идентификатор

2. Примените миграцию

> Миграция в Django — это механизм для управления изменениями в структуре базы данных, связанных с моделями вашего приложения

```shell
py manage.py makemigrations
py manage.py migrate
```

> [!NOTE]
>
> **Задание 1**
>
> Создайте модель для сообществ и мигрируйте её
>
> **Communities**
> |Поле|Тип данных|
> |-|-|
> |name|string(75)|
> |description|string(150)|
> |slug|slug|
> |date|timedate|
> |free|boolean|

---

## Источники

1. [Типы данных в Django](https://docs.djangoproject.com/en/5.1/ref/models/fields/)

---

## 2. Использование админ панели

1. Создание аккаунта администратора и ответьте на вопросы программы создания администратора

```shell
py manage.py createsuperuser
```

2. Авторизируйтесь в [админке](localhost:8000/admin)

## 3. Подключение приложения `posts` к админ панели

1. Добавьте в файл `posts/admin.py`

```python
from django.contrib import admin
from .models import Post

# Register your models here.
admin.site.register(Post)
```

2. Проверьте, что функционал на [админ панели](localhost:8000/admin/) был добавлен

> [!NOTE]
>
> **Задание 2**
>
> Таким же образом подключите приложение сообществ к админ панели

3. Создайте 3 поста на любую тему и 2 группы

> slug — это короткая, человеко-читаемая строка, которая используется в URL вместо идентификаторов или длинных заголовков
> |Название статьи|slug|
> |-|-|
> |Использование Django|django-usage|
> |Модели в Django|modeli-v-django|

## 4. Выводим посты на странице

1. Добавьте в `posts/views.py` код, который будет брать посты из базы данных и сортировать их по дате и передавать их в шаблон `posts_list.html`

```python
from django.shortcuts import render
from .models import Post

def posts_list(request):
    posts = Post.objects.all().order_by('-date')
    return render(request, 'posts/posts_list.html', {'posts': posts})
```

2. Измените шаблон `posts_list.html`

```html
{% extends 'layout.html' %} {% block title %} Posts {% endblock %} {% block
content %}
<section>
    <h1>Posts</h1>

    {% for post in posts %}
    <article class="post">
        <h2>{{ post.title }}</h2>
        <p>{{ post.date }}</p>
        <p>{{ post.body }}</p>
    </article>
    {% endfor %}
</section>
{% endblock %}
```

> [!NOTE]
>
> **Задание 3**
>
> Сделайте так, чтобы группы выводились по адресу `localhost:8000/communities`
