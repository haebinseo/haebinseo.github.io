---
layout: post
title: "Django 개념 및 구조 정리"
date: "2021-01-17 23:50:00"
author: Haebin Seo
categories: django
tags: python django
---
# Django 개념 및 구조 정리

[`Django 명령어`](#django-명령어) [`Django 구조`](#django-구조) [`Model`](#model) [`Admin`](#admin) [`URL dispatcher`](#url-dispatcher) [`View`](#view) [`Template`](#template) [`Form`](#form)

#### Django 명령어
- django project 생성  
  ```shell
  $ django-admin startpoint {project_name}
  ```
- django app 설정  
  settings.py에서 TIMEZONE, static path, ALLOWED_PATH, DB등을 수정

- django project 생성
  ```shell
  $ python managy.py runserver 0:8000
  ```

<br>

<h4 id="django-구조">Django 구조 - MVT Architecture<sup><a href="#footnote-1">[1]</a></sup></h4>

![django_architecture](/assets/django/django_architecture.png)
- URL mapper는 요청 URL을 기준으로 HTTP 요청을 적절한 view로 보낸다.
- view는 HTTP 요청을 수신하고 HTTP 응답을 반환하는 요청처리 함수이다. view는 model을 통해 요청을 충족시키는데 필요한 데이터에 접근한다.
- model은 응용 프로그램의 데이터 구조를 정의하고 데이터베이스의 기록을 관리하고 쿼리하는 방법을 제공하는 Python 객체이다.
- template은 파일의 구조나 레이아웃을 정의하고(예: HTML 페이지), 실제 내용을 보여주는 데 사용되는 플레이스홀더를 가진 텍스트 파일이다. view는 HTML template을 이용하여 동적으로 HTML 페이지를 만들고 model에서 가져온 데이터로 채운다. template이 꼭 HTML 타입일 필요는 없다.

<br>

#### Model
Django의 ORM을 통해 DB의 Relation에 mapping되는 객체이다. models.py에서 class로 선언하고, relation의 각 column들은 class의 property로 mapping된다.  
property들을 django.db.models 경로에서 `CharField`, `DateTimeField` 등의 인스턴스로 초기화한다. `ForeignKey`(1:N), `OneToOneField`(1:1), `ManyToManyField`(M:N) 클래스를 사용해 relation간의 관계(relationship)도 표현해준다. 이는 한쪽의 model에서만 정의해도 작동한다.

models.py를 수정한 이후에는 DB 반영을 위한 migration code를 생성한 뒤 migrate 해줘야 한다.

```shell
$ python manage.py makemigrations {app_name}
$ python manage.py migrate {app_name}
```

<br>

#### Admin
관리자 페이지에서 CRUD 작업을 수행하려면 해당 model을 admin에 추가해줘야 한다. admin.py에 `admin.site.register()` 함수를 사용하면 된다.

- 관리자 페이지를 위한 superuser 생성
  ```shell
  $ python manage.py createsuperuser
  ```

<br>

<h4 id="url-dispatcher">URL dispatcher<sup><a href="#footnote-2">[2]</a></sup></h4>

Django는 URL를 view에 mapping하기 위해 URLconf(URL configuration) 모듈을 사용한다.
urls.py의 urlpatterns에 `django.urls.path()` 혹은 `django.urls.re_path()` 인스턴스를 추가해 해당 url을 처리해줄 수 있다. (url()은 re_path()의 동의어이다)  
path()와 re_path() 함수의 name parameter로 해당 URL에 이름을 mapping할 수 있다.

`django.urls.include()` 함수로 다른 app의 urls.py에 연결할 수 있다.

<br>

#### View
view는 어플리케이션의 비지니스 로직을 작성하는 부분이다.


<br>

#### Template
- 변수 출력
  ```html
  {% raw %}{{ variable }}{% endraw %}
  ```

- for
  ```html
  {% raw %}{% for elem in arr %}
    {{ elem }}
  {% endfor %}{% endraw %}
  ```

- if
  ```html
  {% raw %}{% if condition1 %}
    …
  {% elif condition2 %}
    …
  {% else %}
    …
  {% endif %}{% endraw %}
  ```

- filter
  ```html
  {% raw %}{% filter force_escape|lower %}
    …
  {% endfilter %}{% endraw %}
  ```

  cf) if와 filter를 조합해 사용할 수 있다.
  ```html
  {% raw %}{% if message|length >= 100 %}{% endraw %}
  ```

<br>

#### Form
Model class의 field들이 데이터베이스의 field에 mapping되듯, Form class의 field들은 HTML form의 input element에 mapping된다.
Model의 form class를 통해 Model을 HTML form에 mapping할 수 있다.
또한 Form은 HTML form이 submit되었을 때 그 값을 검증한다.

Form은 submit시 같은 view를 가져온다. 따라서 GET, POST 각 method에 맞는 기능을 제공하도록 view를 작성해야 한다.

- ModelForm 예시
  ```python
  # forms.py
  from django import forms
  from .models import Post

  class PostForm(forms.ModelForm):
    class Meta:
      model = Post
      field = ('title', 'text')
  ```

- Template에 적용
  ```html
  <!-- as_p 혹은 as_table 사용 -->
  {% raw %}{{ form.as_p }}{% endraw %}
  ```

<br>

#### 참조
<a id="footnote-1" href="#django-구조">[1]</a> MVT Architecture - [참고](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Introduction "https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Introduction")

<a id="footnote-2" href="#url-dispatcher">[2]</a> URL dispatcher - [document](https://docs.djangoproject.com/ko/3.1/topics/http/urls "https://docs.djangoproject.com/ko/3.1/topics/http/urls")
