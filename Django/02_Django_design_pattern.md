# 1. django 프로젝트와 앱

## 1-1 django project
- 애플리케이션의 집합 (DB 설정, URL 연결, 전체 앱 설정 등을 처리)


## 1-2 django application
- 독립적으로 작동하는 기능 단위 모듈 (각자 특정한 기능을 담당하며 다른 앱들과 함께 하나의 프로젝트를 구성) => `MTV 패턴에 해당하는 파일 및 폴더를 담당한다.

## :star:만약 블로그를 만든다면
- 프로젝트 : 블로그 (전체 설정 담당) (앱이 모여서 프로젝트를 구성한다.)
- 앱 : 게시글, 댓글, 카테고리 회원 관리 등 (DB, 로직, 화면)

## 1-3 앱 생성
```python
$ python manage.py startapp articles

>>> 앱의 이름은 '복수형'으로 지정하는 것을 권장
```

## 1-4 앱 등록
```python
# settings.py에서

INSTALLED_APPS = [
  # 앱 등록 권장순서
  # 1. local app
  'articles',
  # 2. 3rd party app (설치를 통해 추가하는 앱)
  # 3. 기본 django app
  'django.contrib.admin',
  'django.contrib.auth',
  'django.contrib.contenttypes',
  'django.contrib.sessions',
  'django.contrib.messages',
  'django.contrib.staticfiles',
]

>> 반드시 앱을 생성한 후에 등록해야한다.
```

# 2. django 디자인 패턴

## 2-1 (소프트웨어) 디자인 패턴

- 소프트웨어 설계에서 발생하는 문제를 해결하기 위한 일반적인 해결책 (공통적인 문제를 해결하는데 쓰이는 형식화 된 관행)

## 2-2 MVC 디자인 패턴
### (Model - View - Controller 3등분으로 나누어 개발)

- 애플리케이션을 구조화하는 대표적인 패턴 (데이터, 사용자 인터페이스, 비즈니스 로직을 분리)
- `시각적 요소와 뒤에서 실행되는 로직을 서로 영향 없이, 독립적이고 쉽게 유지보수할 수 있는 애플리케이션을 만들기 위해`

## 2-3 MTV 디자인 패턴
### (Model - Template - View)

- django에서 애플리케이션을 구조화하는 패턴 (기존 MVC 패턴과 동일하나 명칭을 다르게 정의)
- View -> Template
- Controller -> View

## 2-4 프로젝트 구조
- settings.py
  - 프로젝트의 모든 설정을 관리
- urls.py
  - URL과 이에 해당하는 적절한 views를 연결
- __init__.py
  - 해당 폴더를 패키지로 인식하도록 설정
- asgi.py
  - 비동기식 웹 서버와의 연결 관련 설정
- wsgi.py
  - 웹 서버와의 연결 관련 설정
- manage.py
  - Django 프로젝트와 다양한 방법으로 상호작용하는 커맨드라인 유틸리티

## 2-5 앱 구조
- apps.py
  - 앱의 정보가 작성된 곳
- tests.py
  - 프로젝트 테스트 코드를 작성하는 곳

## 정리
- 요청 -> urls.py -> views.py -> models.py / templates -> views.py -> 응답
- 사용자의 `요청`(ex. articles/라는 주소로 요청) -> 해당 주소와 일치하는 `urls.py`가 해당되는 `viws.py`함수로 호출한다. -> views.py(def로 정의한 ex. index) views.py에 있는 `index함수`가 `templates` 하나를 찾아서(ex. index.html)을 가져온다. 그래서 하나의 `렌더링`을 한다. 그 다음 응답을 잘 만들어서 `응답 개체를 return`한다(응답)-> 응답을 브라우저가 받아서 사용자에게 페이지를 해석해서 보여준다.

# 3. 요청과 응답

## 3-1 URLs
```python
# urls.py

from django.contrib import admin
from django.urls import path
# urls.py 입장에서는 articles라는 패키지에서
# views라는 모듈을 가져와야함
from articles import views

urlpatterns = [
  path('admin/', admin.site.urls),
  path('articles/', veiws.index), # 'articles/' 여기에 원하는 것을 넣는다. ex. 'berners-lee/'
]
```
- http://128.0.0.1:8000/`articles/`로 요청이 왔을 때 `views 모듈`의 `index 뷰 함수`를 `호출`한다는 뜻

## 3-2 View
```python
# views.py

from django.shortcuts import render

# Create your views here.
# 특정 기능을 수행하는 view 함수를 만든다.
def index(request):
  return render(request, 'articles/index.html')
  # templates폴더는 기본 조건이다. 그래서 templates폴더 안에 articles 폴더 안에 index.html를 표현할 때 'articles/index.html'라고 한다.
  # return render(응답, '(메인 페이지)')
  # 메인 페이지의 기본 조건은 templates에서 찾는다.
```
- 특정 경로에 있는 `template`과 `request 객체`를 결합해 응답 객체를 반환하는 index 뷰 함수 정의

## 3-3 Template

- 반드시 `templates 폴더명`으로 생성해야 한다.

## 3-4 django에서 template을 인식하는 경로 규칙

- `app폴더 / templates /` articles / index.html
- `app폴더 / templates /` example.html
- django는 강조된 지점까지는 기본 경로로 인식하기 때문에 이 지점 이후의 template 경로를 작성해야 한다.

# :bulb: 데이터 흐름에 따른 코드 작성!
## URLS -> View -> Template

- URLs : path('articles/', `views.index`),
- View : def `index`(request):
            return render(request, `'articles/index.html'`)
- Template : articles/templates/`articles`/`index.html`

# 4. 참고

# render 함수
```python
render(request, template_name, context)

>> 주어진 템플릿을 주어진 컨텍스트 데이터와 결합하고
>> 렌더링 된 텍스트와 함께 HttpResponse(응답) 객체를 반환하는 함수
```
1. request
  - 응답을 생성하는 데 사용되는 요청 객체

2. template_name
  - 템플릿 이름의 경로

3. context
  - 템플릿에서 사용할 데이터 (딕셔너리 타입으로 작성)

## MTV 디자인 패턴 정리
- Model
  - 데이터와 관련된 로직을 관리
  - 응용프로그램의 데이터 구조를 정의하고 데이터베이스의 기록을 관리

- Template
  - 레이아웃과 화면을 처리
  - 화면상의 사용자 인터페이스 구조와 레이아웃을 정의

- View
  - Model & Template과 관련한 로직을 처리해서 응답을 반환
  - 클라이언트의 요청에 대해 처리를 분기하는 역할

- View 예시
  - 데이터가 필요하다면 model에 접근해서 데이터를 가져오고
  - 가져온 데이터를 template로 보내 화면을 구성하고
  - 구성된 화면을 응답으로 만들어 클라이언트에게 반환