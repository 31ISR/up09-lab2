# Лабораторная работа 2

## Дедлайн сдачи работы без пенальти

10.12.2024 00:00

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
{% extends 'layout.html' %} {% block title %} Posts {% endblock %}
{% block content %}
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

## 5. Добавляем название страницы

1. Добавьте название для страницы постов в `posts/urls.py`

```python
urlpatterns = [
    path('', views.posts_list, name="posts"),
]
```

2. Замените ссылку на страницу posts в html на `{% url 'posts' %}`

> [!NOTE]
>
> **Задание 4**
>
> Сделайте так, чтобы к переход к группам также происходил по `name`, а не прямому URL

---

### Источники

-   [Django преобразование URL](https://docs.djangoproject.com/en/5.1/topics/http/urls/)

---

3. Добавляем slug для использования в адресах постов в `posts/urls.py`

```python
urlpatterns = [
    path('', views.posts_list, name="posts"),
    path('<slug:slug>', views.post_page, name="page")
]
```

4. Добавляем маршрут slug маршрут для постов в `posts/views.py`

```python
def post_page(request, slug):
    post = Post.objects.get(slug=slug)
    return render(request, 'posts/post_page.html', {'post': post})
```

5. Добавить ссылку к заголовку поста в шаблоне списка постов

```html
<h2>
    <a href="{%  url 'posts:page' slug=post.slug %}"> {{ post.title }} </a>
</h2>
```

6. Добавьте название для ссылок в `posts/urls.py`, чтобы разграничивать маршруты приложений

```python
...
from . import views

app_name = 'posts'

urlpatterns = [
...
```

> [!WARNING]
>
> Используйте это названия в ссылках, которые ведут на приложение связанное с постами
>
> `layout.html`
>
> ```html
> <a href="{% url 'posts:list' %}"> ... </a>
> ```
>
> Обновите название для списка постов в `posts/urls.py`
>
> `posts/urls.py`
>
> ```python
> path('', views.posts_list, name="list"),
> ```

7. Создайте шаблон отображения `posts/templates/posts/post_page.html` для отображения поста

```html
{% extends 'layout.html' %} {% block title %} {{ post.title }} {% endblock %}
{% block content %}
<section>
    <h1>{{ post.title }}</h1>
    <p>{{ post.date }}</p>
    <p>{{ post.body }}</p>
</section>
{% endblock %}
```

> [!NOTE]
>
> **Создайте такой же функционал для сообществ**
>
> -   Список с сообществами
> -   При нажатии на сообщество должен быть переход на страницу сообщества
> -   На странице сообщества должно показываться название сообщества и его краткое описание
