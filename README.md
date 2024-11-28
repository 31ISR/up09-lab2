# Лабораторная работа 2

## Как выполнять

Сделать форк лабораторной работы один, поменять название на `up09-lab2-{ваша_фамилия}`

## 1. Создание схемы для приложения posts

1. В файле `posts/models.py` создайте модель Post, которая описывает посты

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=75)
    body = models.TextField()
    slug = models.SlugField()
    date = models.DateTimeField(auto_now_add=True)
```

2. Примените миграцию

> Миграция в Django — это механизм для управления изменениями в структуре базы данных, связанных с моделями вашего приложения

```shell
py manage.py makemigrations
py manage.py migrate
```

> [!NOTE]
> Создайте модель для сообществ и мигрируйте её
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
