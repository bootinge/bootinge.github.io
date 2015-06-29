---
layout: default
title: Django Basic
---
---------------
# Django 웹 프레임워크

## High-level Python Web Framework
- 웹 페이지를 만들기 위한 대부분의 요소가 포함되어 있음.
- HTTP Request / Response 처리, URL 패칭
- 템플릿 엔진
- ORM
- Follow best practices

# Django의 역할
- Web Application의 역할
![Django Role 1](images/role_1.png)

- Django의 역할
![Django Role 2](images/role_2.png)

# Model-View-Template 구조
## Model
- Represents your data
- Each model class represents a database table.

## View(MVC의 Controller에 해당)
- Takes HTTP request and returns response
- May use model to retrieve/store data
- May call a template to represent data

## Template(MVC의 View에 해당)
- Generates HTML
- Presentation login only

# Django 실습
## 프로젝트 생성.
- django를 설치하고 나면, bin폴더에 django-admin.py이 생성됩니다.
- 프로젝트를 생성하기 위해서 다음과 같이 명령을 호출합니다.
```
$ django-admin.py startproject mysite
```

## 프로젝트 구조.
```
mysite/
    __init__.py
    manage.py
    settings.py
    urls.py
```

- \_\_init\_\_.py : 파이썬에게 이 디렉토리를 파이썬 패키지로 해석하도록 지시하기 위한 비어있는 파일입니다.
- manage.py: 다양한 방법으로 Django 프로젝트를 관리할 수 있게 하는 명령줄 유틸리티 입니다.
- settings.py: 이 Django 프로젝트의 설정입니다.
- urls.py: 이 Django 프로젝트에서 사용할 URL 설정을 담고 있습니다.

## urls.py
- URL Dispatcher
```
from django.conf.urls.defaults import *
urlpatterns = patterns('mysite.views',
    (r'^hello/$', 'hello'),
    (r'^time/$', 'current_datetime'),
    (r'^time/plus/(\d{1,2})/$', 'hours_ahead'),
)
urlpatterns += patterns('weblog.views',
    (r'^tag/(\w+)/$', 'tag'),
)
```
- URL 패턴 정의는 Regular Expression을 따릅니다.


## 서버 구동
- 서버는 다음과 같이 구동합니다.
```
$ python manage.py runserver 8080
```
```
Validating models...
    0 errors found.

    Django version 0.95, using settings 'mysite.settings'
    Development server is running at http://127.0.0.1:8000/
    Quit the server with CONTROL-C (Unix) or CTRL-BREAK (Windows).
```

- 웹 브라우저로 http://127.0.0.1:8080에 접속하세요.

## startapp
- polls app을 만들기 위해서 다음과 같이 명령을 호출합니다.
```
$ python manage.py startapp polls
```
- 명령 수행 결과 디렉토리 구조는 다음과 같습니다.
```
polls/
    __init__.py
    admin.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```
- polls/admin.py : Admin 사이트에 모델 클래스를 등록해주는 파일입니다.
- polls/models.py : ORM 형태로 모델을 정의합니다.
- polls/views.py : 뷰 함수를 정의하는 파일입니다.

## 실제 어플리케이션 개발하기.
- Django 공식 홈페이지의 *Writing your first Django app*을 따라서 해봅시다.
- https://docs.djangoproject.com/en/1.8/intro/tutorial01/

# Django best practices
## Openstack Horizon

### 소개
- A dashboard for managing Openstack Services
- Powered by Django

---------------

# 서비스 Architecture
- 실제 서비스에 사용하는 Architecture

## Instagram
### Functionality
- Smartphone photo sharing
- Post to other social networks
- Send messages

### Sortware architecture
- Python, Django
- PostgreSQL
- Redis
- Nginx
- Node.js
- Android

### Data Center
- started with single small scale PC
- 100+ instances at Amazon

# System Architecutre
![Instagram Architecture](images/instagram.png)

## wsgi?
- Specification for an interface between web servers and Python web applications or frameworks.
- Intended to promote web application portability across a variety of web servers.

## mod_wsgi?
- An Apache module to support hosting WSGI applications in conjunction with the Apache web server.
- Intercepts requests to designated URLs and passes requests off to the WSGI application specified in the target WSGI script file that the URL maps to.

## Apache와 연동하기
### wsgi.py 파일 생성.
- Application 폴더에 wsgi.py 파일을 생성합니다.
- 경로 설정에 주의하세요.
```
import os, sys
path = os.path.abspath(\_\_file\_\_+'/../..')
if path not in sys.path:
        sys.path.append(path)
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "frontend.settings")
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

### Apache 설정 변경.
- httpd.conf 를 변경합니다.
```
LoadModule wsgi_module modules/mod_wsgi.so
```

- Mapping WSGI application at root of web server.
```
  WSGIScriptAlias / /home/grumpy/example-1/wsgi.py
```

- Mapping WSGI application at sub URL of web server.
```
  WSGIScriptAlias /suburl /home/grumpy/example-1/wsgi.py
```

- Access 허용하기(httpd.conf)
```
<Directory /home/grumpy/example-1> Order deny,allow
Allow from all
</Directory>
```

# 참고
![Lanyrds Architecture](images/lanyrds-architecture.jpg)
