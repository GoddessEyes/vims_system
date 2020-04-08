# TV

!!!
## TV

!!!
## Templates - Views

!!!
## Views
- Это функции

!!!
## Views
- Это функции
- Или классы

!!!
## Views: суть
- Принимают запрос - отдают ответ

!!!
## Views: суть
- Принимают запрос

!!!
## Views: суть
- Принимают запрос - КАК?!

!!!
## Views
![requests](./images/request.png)

!!!
## Разбираемся
- Куда пришел запрос и что происходит?

!!!
## Data flow

!!!
## Data flow
- WSGI

!!!
## Data flow
- WSGI
- Middleware

!!!
## Data flow
- WSGI
- Middleware
- View

!!!
## WSGI
Прослойка между веб-сервером и веб-приложением

!!!
## Middleware
- Цепочка фильтров

!!!
## Middleware
- Могут обрабатывать поток данных как в одном направлении, так и в обратном

!!!
## Middleware
- Это класс

!!!
## Middleware
- Это класс
- Реализуют некоторые из методов:
- ```process_request```
- ```process_view```
- ```process_exception```
- ```process_response```

!!!
## ```process_request```
- Обрабатывает HTTP-запрос, возвращает объект HttpRequest

!!!
## Остальные методы
- Обрабатывают поведение views

!!!
## Как понимать куда уйдет запрос?

!!!
## Как понимать куда уйдет запрос?
- Url dispatcher или ```UrlConf```

!!!
## UrlConf
```python
from django.urls import path

from . import views

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<int:year>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
    path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
]
```
!!!
## ```process_view```
- Список аргументов ```process_view(request, view_function, view_args, view_kwargs)```

!!!
## Middleware
- Если возвращает ```None``` - вызов следующей
- Если возращает ```HttpResponse``` завершаются

!!!
## Views
```python
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```
!!!
## Http
```python
import django.http
```
!!!
## Http
- А что если у нас есть html-документ?

!!!
## TemplateResponse
```python
from django.template.response import TemplateResponse

def blog_index(request):
    return TemplateResponse(request, 'entry_list.html', {'entries': Entry.objects.all()})
```
!!!
## TemplateResponse
- Перед тем как возращается instance объекта, происходит рендеринг

!!!
## TemplateResponse
- Перед тем как возращается instance объекта, происходит рендеринг
- Весь контент будет лежать в TemplateResponse.content

!!!
## Однако
- Рендеринг произойдет один раз

!!!
## Однако
```python
>>> from django.template.response import TemplateResponse
>>> t = TemplateResponse(request, 'original.html', {})
>>> t.render()
>>> print(t.content)
Original content

>>> t.template_name = 'new.html'
>>> t.render()
>>> print(t.content)
Original content
```
!!!
## Как решать?!

!!!
## Как решать?!
- Использовать ```django.shortcut```

!!!
## render()
```python
from django.shortcuts import render

def my_view(request):
    # View code here...
    return render(request, 'myapp/index.html', {
        'foo': 'bar',
    }, content_type='application/xhtml+xml')
```
!!!
## Аналогично
```python
from django.http import HttpResponse
from django.template import loader

def my_view(request):
    # Что-то сделали...
    t = loader.get_template('myapp/index.html')
    c = {'foo': 'bar'}
    return HttpResponse(t.render(c, request), content_type='application/xhtml+xml')
```
!!!
## Время практики

!!!
