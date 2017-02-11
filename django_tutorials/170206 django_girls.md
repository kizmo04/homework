# Django girls

## 초기 설정 

가상환경 만들기
pyenv virtualenv 3.4.3 django_girls

가상환경 정해주기
pyenv local django_girls

쟝고 설치해주기
pip install django 


requirements.txt 만들기
pip freeze > requirements.txt

.gitignore 만들기

깃 설정

## 프로젝트 시작 

django-admin startproject mysite


## 설정변경 - settings.py

time_zone 변경 'Asia/Seoul'

STATIC_ROOT = os.path.join(BASE_DIR, 'static')
>이 설정은 처음해본듯?

ipython, django-extensions 설치 후에 installed_apps에 추가
'django_extensions',

## 데이터베이스 설정 

python manage.py migrate


서버돌리기
manage.py runserver

manage.py startapp blog


installed_apps에 'blog.apps.BlogConfig', 추가

 
## 모델 만들기 
blog/models.py

클래스와 메소드 만들기, __str__메소드

manage.py makemigrations blog

manage.py migrate blog

## url conf


## ORM 
![](images/스크린샷 2017-02-07 오전 4.48.49.png)