# djago tutorial - 설문조사 앱 만들기

## 설치하기

먼저 가상환경을 생성해두어야 한다.

```shell
pyenv virtualenv 3.4.3 django_tutorial

pyenv local django_tutorial
```
가상환경을 만들고 지정해준다. (파이썬 3.4.3버전을 권장함)

그리고 공식 버전의 장고를 설치한다

```shell
pip install django
```

설치가 잘 되었는지 확인하기

```python
In [1]: import django

In [2]: print(django.get_version())
1.10.5
```

## 파트1 
튜토리얼을 통해 설문조사 어플리케이션을 만든다. 관리자용 페이지와 사용자용 페이지 부분으로 나눠서 만들것이다. 

> `python -m django --version`
> 으로 쟝고의 버전을 확인해본다. 이 문서는 Django 1.10	을 기준으로 작성되었다.
> 

### 프로젝트 생성 
코드를 저장할 디렉토리로 이동 후 다음 커맨드를 실행한다.
```shell
django-admin startproject mysite
```
`ls`로 확인해보면 `mysite`라는 디렉토리가 생성된 것을 확인할 수 있다.

```shell
(django_tutorial) ➜  tutorial git:(master) ✗ tree mysite
mysite
├── manage.py
└── mysite
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py

1 directory, 5 files
```
안에는 5개의 파일이 생성되어있다.

* mysite/ 디렉토리 바깥의 디렉토리는 단순히 프로젝트를 담는 공간입니다. 이 이름은 Django 와 아무 상관이 없으니, 원하는 이름으로 변경하셔도 됩니다.

* manage.py: Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티 입니다. manage.py 에 대한 자세한 정보는 django-admin and manage.py 에서 확인할 수 있습니다.

* mysite/ 디렉토리 내부에는 project 를 위한 실제 Python 패키지들이 저장됩니다. 이 디렉토리 내의 이름을 이용하여, (mysite.urls 와 같은 식으로) project 어디서나 Python 패키지들을 import 할 수 있습니다.

* mysite/__init__.py: Python 으로 하여금 이 디렉토리를 패키지 처럼 다루라고 알려주는 용도의 단순한 빈 파일입니다. Python 초심자라면, Python 공식 홈페이지의 more about packages 를 읽어보십시요.
 
* mysite/settings.py: 현재 Django project 의 환경/구성을 저장합니다. Django settings 에서 환경 설정이 어떻게 동작하는지 확인할 수 있습니다.
 
* mysite/urls.py: 현재 Django project 의 URL 선언을 저장합니다. Django 로 작성된 사이트의 “목차” 라고 할 수 있습니다. URL dispatcher 에서 URL 에 대한 자세한 내용을 읽어보세요.
 
* mysite/wsgi.py: 현재 project 를 서비스 하기 위한 WSGI 호환 웹 서버의 진입점 입니다. How to deploy with WSGI 를 읽어보세요.

### 개발서버 
Django에는 개발 서버가 포함되어 있다. manage.py가 있는 디렉토리에서 다음 명령으로 확인해본다. 
```shell
python manage.py runserver
```
다음과 같은 내용이 나온다.

```sehll
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

February 03, 2017 - 15:50:53
Django version 1.10, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
>포트 변경하기
기본적으로, runserver 명령은 내부 IP 의 8000 번 포트로 개발 서버를 띄웁니다.
>만약 이 서버의 포트를 변경하고 싶다면, 커맨드라인에서 인수를 전달해주면 됩니다. 예를들어, 이 명령은 포트를 8080 으로 서버를 시작할 것입니다.
`$ python manage.py runserver 8080`
>만약 서버의 IP 를 바꾸고 싶다면, 포트와 함께 전달해 주면 됩니다. 다음과 같이 내 컴퓨터에 연결된 모든 공개 IP 를 서버에 연결(listen) 할 수 있습니다. 네트워크 상의 다른 컴퓨터에게 내 작업물을 보여줄 때 유용합니다.
`$ python manage.py runserver 0.0.0.0:8000`
>개발 서버에 대한 자세한 내용은 runserver 를 참조하세요.



>runserver 의 자동 변경 기능
개발 서버는 요청이 들어올 때마다 자동으로 Python 코드를 다시 불러옵니다. 코드의 변경사항을 반영하기 위해서 굳이 서버를 재기동 하지 않아도 됩니다. 그러나, 파일을 추가하는 등의 몇몇의 동작은 개발서버가 자동으로 인식하지 못하기 때문에, 이런 상황에서는 서버를 재기동 해야 적용됩니다.

이제 환경설정이 끝났다!

### 설문조사 앱 만들기 
Django는 앱 기본 디렉토리 구조를 자동으로 생성해준다. 여기서는 탑 레벨에서 import 할 수 있게 manage.py와 같은 레벨에서 앱을 생성한다.

```shell
python manage.py startapp polls
```
poll 어플리케이션의 디렉토리 구조가 생겼다.

```shell
(django_tutorial) ➜  mysite git:(master) ✗ tree polls
polls
├── __init__.py
├── admin.py
├── apps.py
├── migrations
│   └── __init__.py
├── models.py
├── tests.py
└── views.py

1 directory, 7 files
```

### 첫번째 뷰 작성하기 
`polls/view.py` 에 다음과 같은 reponse를 보내주는 코드를 작성한다.
```python
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

view를 호출하기 위한 url은 URLconf가 매핑해준다. `polls/urls.py`에 다음과 같이 작성해준다.

```python
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```

이제 프로젝트 최상단의 URLconf에서 `polls`의 URLconf를 참조할 수 있게 `mysite/urls.py`에 다음과 같이 작성한다.

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```



이제 `polls/views.py`에 작성한 index뷰가 URLconf에서 연걸되었다. 작동확인은 
`python manage.py runserver`
를 실행하고 브라우저에서 서버 주소에 /polls 접속해보면 된다.
`http://127.0.0.1:8000/polls`

### 예제에서 사용한 메소드들 

#### [include()](http://django-document-korean.readthedocs.io/ko/latest/ref/urls.html#include)
Django는 include()를 만나는 시점까지의 URL을 잘라내고 남은 부분을 include된 URLconf로 전달한다. 즉, `/polls/some/method` 를 요청받으면, `some/method` 가 `polls/urls.py` 의 URLconf 로 넘어간다. 이것은 `/polls/`, 혹은 `/fun_polls/`, 혹은 `/content/polls/` 같은 그 어떤 경로에 붙이더라도 동작한다. admin.site.urls 를 제외한, 다른 URL 패턴을 include 할 때마다 항상 include() 를 사용해야 한다.

include(module, namespace=None, app_name=None)


include(pattern_list)


include((pattern_list, app_namespace), namespace=None)


include((pattern_list, app_namespace, instance_namespace))

* module – URLconf module (or module name)
* namespace (string) – Instance namespace for the URL entries being included
* app_name (string) – Application namespace for the URL entries being included
* pattern_list – Iterable of django.conf.urls.url() instances
* app_namespace (string) – Application namespace for the URL entries being included
* instance_namespace (string) – Instance namespace for the URL entries being included


#### [url()](http://django-document-korean.readthedocs.io/ko/latest/ref/urls.html#url)

url(regex, view, kwargs=None, name=None)

url() 함수에는 4 개의 인수가 전달되었습니다. 두개의 필수 인수로 regex 와 view 가 있고, 두개의 옵션 인수로 kwargs 와 name 이 있습니다. 이 시점에서, 이 인수가 무엇인지 살펴보는것은 의미가 있습니다.

url() 인수: regex
“regex” 는 보통 정규 표현식(“Regular Expression”) 을 짧게 줄여 쓰는 표현입니다. 문자열의 패턴을 일치시키는 문법을 말하며, 이 경우에는 url 의 패턴을 찾아내는데 사용되었습니다. Django 에서는 목록의 첫번째 정규표현식부터 시작해서, 요청된 URL 에 대하여 일치하는 정규 표현식이 발견 될때까지 차례로 비교합니다.

이 정규 표현식 들은 GET 이나 POST 의 매개변수나, 도메인 이름을 뒤지지는 않습니다. 예를 들어, https://www.example.com/myapp/ 가 요청된 경우, URLconf 는 오직 myapp/ 부분만 바라봅니다. https://www.example.com/myapp/?page=3 같은 요청에도, URLconf 는 역시 myapp/ 부분만 신경씁니다.

정규 표현식에 대해 도움이 필요하다면, Wikipedia’s entry 를 참고하시거나 re 모듈의 문서를 참고해 주십시요. 특히 Jeffrey Friedl 가 쓴 오라일리 출판의 “정규표현식 완전 해부와 실습”(“Mastering Regular Expressions”) 는 완벽한 참고서입니다.그러나 현실적으로 이 모든 내용을 전부 알아야 할 필요는 없습니다. 딱 필요한 만큼, 간단한 패턴을 잡아낼 정도만 알면 됩니다. 사실, 복잡한 정규표현식을 사용하면 검색 속도가 아주 느려지므로, 전적으로 정규 표현식에 의존하지 않아야 합니다.

마지막으로 성능에 관해 알아야 할 사실은, 이런 정규 표현식들은 URLconf 모듈이 처음 불러올 때 자동으로 컴파일 되기 때문에 엄청나게 빠르다는 것입니다. 물론 앞서 언급했듯이 복잡한 검색을 쓰지 않는 한 말이죠.

url() 인수: view
Django 에서 일치하는 정규 표현식을 찾아내면, HttpRequest 객체를 첫번째 인수로 하고, 정규표현식에서 “잡힌” 값들을 나머지 인수로 하여 특정한 view 함수를 호출합니다. 만약 정규표현식이 간단한 형식이라면, 잡힌 값들은 단순히 순서 기반의 인수로서 함수에 넘겨집니다. 만약 이름 기반의 정규표현식이라면, 잡힌 값들은 키워드 인수들로 함수에 넘겨집니다. 나중에 이에 대한 간단한 예제를 살펴보겠습니다.

url() 인수: kwargs
임의의 키워드 인수들은 목표한 view 에 사전형으로 전달됩니다. 그러나 이 튜토리얼에서는 사용하지 않을겁니다.

url() 인수: name
URL 에 이름을 지으면, 템플릿을 포함한 Django 어디에서나 명확하게 참조할 수 있습니다. 이 강력한 기능을 이용하여, 단 하나의 파일만 수정해도 project 내의 모든 URL 패턴을 바꿀 수 있도록 도와줍니다.


## 파트2 - DB설치와 모델생성 
### `mysite/settings.py` 
Django의 설정을 변수로 표현한 모듈. 데이터베이스를 비롯한 여러가지 설정이 있다. 

### [DATABASES](http://django-document-korean.readthedocs.io/ko/latest/ref/settings.html#std:setting-DATABASES)

데이터베이스변수의 딕셔너리의 디폴트 값이 sqlite로 되어있다. SQlite는 파이썬에서 기본으로 제공된다.

* ENGINE – 'django.db.backends.sqlite3', 'django.db.backends.postgresql', 'django.db.backends.mysql', or 'django.db.backends.oracle'. 그 외에 서드파티 백엔드 를 참조.

* NAME – 데이터베이스의 이름. 만약 SQLite 를 사용 중이라면, 데이터베이스는 당신의 컴퓨터의 파일로서 저장됩니다. 이 경우, NAME 는 파일명을 포함한 절대 경로 로서 지정되어야 합니다.기본 값은 os.path.join(BASE_DIR, 'db.sqlite3') 로 정의되어 있으며, project 디렉토리 내에 db.sqlite3 파일로 저장됩니다.

>SQLite가 아닌 데이터베이스라면
만약 SQLite 이외의 데이터베이스를 사용하는 경우, 이 시점에서 데이터베이스를 생성해야 합니다. 데이터베이스의 대화형 프롬프트 내에서 “CREATE DATABASE database_name;” 명령을 실행하면 됩니다.

>또한, mysite/settings.py 에 설정된 데이터베이스 사용자가 “create database” 권한이 있는지도 확인해 봐야 합니다. 튜토리얼을 진행하며 필요한 경우 테스트 데이터베이스 를 자동으로 생성할 수 있도록 해줍니다.
>
> USER, PASSWORD, HOST 같은 추가 설정이 반드시 필요합니다. 더 자세한 내용은 [DATABASES](http://django-document-korean.readthedocs.io/ko/latest/ref/settings.html#std:setting-DATABASES) 문서를 참조
> 

####  [INSTALLED_APPS](http://django-document-korean.readthedocs.io/ko/latest/ref/settings.html#std:setting-INSTALLED_APPS)

현재 Django 인스턴스에서 활성화된 모든 Django 어플리케이션들의 이름이 담겨 있습니다. App 들은 다수의 project 에서 사용될 수 있고, 다른 project 에서 쉽게 사용 될 수 있도록 패키지 하여 배포할 수 있습니다.

기본적으로는, INSTALLED_APPS 는 Django 와 함께 딸려오는 다음의 app 들을 포함합니다.

* django.contrib.admin – 관리용 사이트, 곧 사용하게 될겁니다.

* django.contrib.auth – 인증 시스템.

* django.contrib.contenttypes – 컨텐츠 타입을 위한 프레임워크.
 
* django.contrib.sessions – 세션 프레임워크.
 
* django.contrib.messages – 메세징 프레임워크.

* django.contrib.staticfiles – 정적 파일을 관리하는 프레임워크. 

필요 없는 어플리케이션은 주석처리하면 된다.

#### migrate 
INSTALLED_APPS에 의해 설치되는 어플리케이션을 사용하기 위해 데이터베이스 테이블이 필요하다. 
```shell
python manage.py migrate
```
이 명령을 실행하면  INSTALLED_APPS 의 설정을 탐색하여, mysite/settings.py 의 데이터베이스 설정과 app 과 함께 제공되는 데이터베이스 migrations에 따라, 필요한 데이터베이스 테이블을 생성한다. 


### 모델만들기 
모델이란 부가적인 메타데이터를 가진 데이터베이스의 구조를 뜻한다. Question 과 Choice 라는 두개의 모델을 만들어 보겠습니다. Question 은 질문(question) 과 발행일(publication date) 을 위한 두개의 필드를 가집니다. Choice 는 선택지(choice) 와 표(vote) 계산을 위한 두개의 필드를 가집니다. 각 Choice 모델은 Question 모델과 연관(associated) 됩니다.
 `polls/models.py `에 다음과 같은 코드를 추가한다. 
 
 ```python
 from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published') 


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    
 ```
데이터베이스의 각 필드는 Field클래스의 인스턴스로 표현된다.  
CharField 는 문자(character) 필드를 표현하고, DateTimeField 는 날짜와 시간(datetime) 필드를 표현합니다. 이것은 각 필드가 어떤 자료형을 가질 수 있는지를 Django 에게 말해줍니다. 