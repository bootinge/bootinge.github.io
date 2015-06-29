---
layout: default
title: Django Registration and authentication
---

-------
# Django Registration 구현
- Django에서 Registraion을 구현하는 방법을 설명
- 기본적인 회원 등록, E-mail Verification, 로그인 기능들을 제공함.
  - Django Allauth 모듈을 사용함
  - AllAuth에서는 Facebook, Google 등 다양한 서비스에 대한  OAuth 로그인을 제공함.
- JWT(JSon Web Token) 기반의 인증 처리.
  - django-rest-framework-jwt 사용.

# Registration Form 만들기
## Django Allauth 설치
- 폴더 내부에 example이 있음.

```
$ git clone git://github.com/pennersr/django-allauth.git
$ cd django-allauth/example
$ virtualenv venv
$ . venv/bin/activate
$ pip install -r requirements.txt
```
- DB 생성하기

```
$ python manage.py syncdb
```

## URL 설정하기
- Allauth에서 사용하는 URL을 설정.
- example/urls.py에 설정.

```
urlpatterns = patterns('',
                       (r'^accounts/', include('allauth.urls')),
                       ...
)
```

## SMTP 서버 테스트하기
- Python의 기본 모듈을 통해서 임의의 SMTP 서버를 띄울 수 있음.
  - 아래 명령어는 1025번 포트로 smtp 서버를 띄움

```
$ python -m smtpd -n -c DebuggingServer localhost:1025
```

- settings.py에서 localhost, 1025번 포트로 설정을 변경하고 테스트.

```
EMAIL_HOST='localhost'
EMAIL_PORT=1025
EMAIL_HOST_USER=''
EMAIL_HOST_PASSWORD=''
```

## Real SMTP 서버 설정하기
- 사용자가 회원 가입한 e-mail verification 서버 변경.
- SMTP 서버 정보를 변경해야 함.
- example/settings.py 내부 설정 변경
.

```
$ pip install django_smtp_ssl
```

- 사용할 SMTP 서버 정보 설정.

```
EMAIL_BACKEND = 'django_smtp_ssl.SSLEmailBackend'
EMAIL_USE_TLS = True
EMAIL_HOST = 'smtp.daum.net'
EMAIL_PORT = 465
EMAIL_HOST_USER = 'ADMIN_ID'
EMAIL_HOST_PASSWORD = 'ADMIN_PASSWORD'
DEFAULT_FROM_EMAIL = 'Verified-email <admin@localhost>'
```

> SSL을 사용하는 SMTP 서버는 django.core.mail.backends.smtp.EmailBackend를 사용하면 동작하지 않았음.

## 실행하기

```
$ python manage.py runserver
```

> 여기까지 진행하면, 기본적인 회원 Registration, Login은 가능함.

-------

# JWT(JSON Web Token) 설정
## 설치하기
- Django Rest Framework 및 JWT 모듈 설치.

```
$ pip install djangorestframework
$ pip install djangorestframework-jwt
```

## 설정하기
- Rest Framework에 대한 설정(settings.py)

```
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
        'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
    ),
}
```

- INSTALLED APPS에 Rest Framework 추가.

```
INSTALLED_APPS = (
  ...
  'rest_framework',
  ...
)
```

- URL 및 View 설정하기.

```
from django.contrib.auth.models import User
from rest_framework import routers, serializers, viewsets

# Serializers define the API representation.
class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ('url', 'username', 'email', 'is_staff')

# ViewSets define the view behavior.
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer

# Routers provide an easy way of automatically determining the URL conf.
router = routers.DefaultRouter()
router.register(r'users', UserViewSet)
```

- example/urls.py의 urlpatterns에 추가.

```
url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework')),
url(r'^api-token-auth/', 'rest_framework_jwt.views.obtain_jwt_token'),
url(r'^api-token-refresh/', 'rest_framework_jwt.views.refresh_jwt_token'),
url(r'^api-token-verify/', 'rest_framework_jwt.views.verify_jwt_token'),
```

## 테스트하기
### Login.
- JSON으로 ID와 PW를 전달하면 JSON Web Token를 얻을 수 있음.

```
$ curl -X POST -H "Content-Type: application/json" -d '{"username":"admin","password":"admin"}' http://127.0.0.1:8000/api-token-auth/
```

- 실행 결과 얻어진 JSON Web Token

```
{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwidXNlcl9pZCI6MSwiZW1haWwiOiJhZG1pbkBuYXZlci5jb20iLCJleHAiOjE0MzU1MDY3NzF9.dsqiJDuJPCifRm0wt3qCdgQnuZSZYvIDeZQmCH62D1A"}%
```

### Token Verification
- 얻은 JWT가 유효한지를 체크.
  - 정상적으로 호출됐으면 호출한 Token을 반환함.

```
$ curl -X POST -H "Content-Type: application/json" -d '{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwidXNlcl9pZCI6MSwiZW1haWwiOiJhZG1pbkBuYXZlci5jb20iLCJleHAiOjE0MzU1MDY3NzF9.dsqiJDuJPCifRm0wt3qCdgQnuZSZYvIDeZQmCH62D1A"}' http://127.0.0.1:8000/api-token-verify/
```

### 접근 제한된 URL 추가하기.

- example/demo/views.py에 추가.

```
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated

from rest_framework_jwt.authentication import JSONWebTokenAuthentication

class RestrictedView(APIView):
    permission_classes = (IsAuthenticated, )
    authentication_classes = (JSONWebTokenAuthentication, )

    def get(self, request):
        data = {
            'id': request.user.id,
            'username': request.user.username,
            'token': str(request.auth)
        }
        return Response(data)

```

- example/urls.py의 urlpatterns에 추가.

```
from example.demo.views import RestrictedView
...
urlpatterns = patterns('',
  ...
  url(r'^restricted/$', RestrictedView.as_view()),
  ...
)
```

- 테스트하기

```
$ curl -H "Authorization: JWT eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwidXNlcl9pZCI6MSwiZW1haWwiOiJhZG1pbkBuYXZlci5jb20iLCJleHAiOjE0MzU1MDgwNjJ9.Zd9MSdA41HYJTAjW7JEMsK3TUv5EYXAj5X0S1IdKwFY" http://127.0.0.1:8000/restricted/

```
