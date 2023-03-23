# 1. 개요

## URL dispatcher(운항 관리자, 분배기)

- URL 패턴을 정의하고 해당 패턴이 일치하는 요청을 처리할 view 함수를 연결(매핑)

# 2. 변수와 URL

## Variable Routing
- URL 일부에 변수를 포함시키는 것 (변수는 view 함수의 인자로 전달할 수 있음)

## Variable routing 작성법
- 변수는 "< >"에 정의하며 view함수의 인자로 할당된다.
- 기본 타입은 string이며, 5가지 타입으로 명시할 수 있다.
> <path_converter:variable_name>
- urls의 num이라는 변수명이
> path('articles/<int:num>/', views.detail),
- 정의할 때 인자인 num과 일치해야한다.
> def detail(request, num):

## urls.py
```python
path('articles/<int:num>/', views.hello),
path('hello/<str:name>/', views.greeting),
path('price/<str:thing>/<int:cnt>', views.price),
```
## views.py
```python
# 숫자 하나
def number_print(request, num):
    context = {
        'num' : num,
    }
    return render(request, 'articles/number_print.html', context)


# 숫자를 두개 받아서
def calculate(request, num1, num2):
    context = {
        'plusnum':num1+num2,
        'minus':num1-num2,
        'multi' : num1*num2,
        'num': num1//num2,
    }
    return render(request, 'articles/calculate.html', context)
```
## html
```html
{% extends 'articles/base.html' %}

{% block content %}
  <div><h1>더하기 : {{plusnum}}</h1></div>
  <div><h1>빼기 : {{minus}}</h1></div>
  <div><h1>곱하기 : {{multi}}</h1></div>
  <div><h1>몫 : {{num}}</h1></div>
{% endblock content %}
```

# 3. App의 URL

## App URL mapping
- 각 앱에 URL을 정의하는 것
- 프로젝트와 각각의 앱이 URL을 나누어 관리하여 주소 관리를 편하게 하기 위함

## :bulb: 두 번째 앱 생성 후 URL 주소가 겹친다면..
- 아래와 같이 관리하는 건 별로다.
```python
# fistpjt/urls.py

from articles import views as articles_views
from pages import views as pages_views

urlpatterns = [
  ...,
  path('pages-index', pages_views.index),
]
```
## :star: URL을 각자 app에서 관리하자!!
  - 기존에는 requests받아 urls에서 각 앱의 views로 요청을 했는 데
  - requests받아 urls 요청 값을 각 앱의 urls을 생성해 각자의 urls로 views의 값을 요청한다.

### 예시
- 프로젝트 폴더의 urls.py파일
```python
# firstpjt/urls.py

from django.contrib import admin
from django.urls import path, include
# from articles import views
urlpatterns = [
  path('admin/', admin.site.urls),

  path('articles/', include('articles.urls')),
  path('', include('articles.urls')),
]
```
- articles앱의 urls.py
```python
# articles/urls.py

from django.urls import path
from . import views
# import views만 작성해도 되는데 from . (현재위치를 나타내는)까지 명시적 상대경로 (django 권장사항)

urlpatterns = [
  path('index/', views.index),
  path('throw/', views.throw),
  path('catch/', views.catch),
  path('number-print/<int:num>/', views.number_print),
  path('', views.index),
  path('calculate/<int:num1>/<int:num2>/', views.calculate),
]
```
- pages앱의 urls.py
```python
# pages/urls.py

from django.urls import path
from . import views
urlpatterns = [
  path('index/', views.index),
  path('hello/<str:name>/', views.greeting)
]
```

## include()
- 다른 URL들을 참조할 수 있도록 돕는 함수
- (URL의 그 시점까지 일치하는 부분을 `잘라내고`, 남은 문자열 부분을 후속 처리를 위해 include된 URL로 `전달`한다.)

# 4. URL 이름 지정
- 기존 'articles/' 주소가 'articles/index/'로 변경됨
```python
# firstpjt/urls.py

path('articles/', include('articles.urls'))
```
```python
# articles/urls.py

path('index/', views.index, name='index')
```
- 기존에 articles/ 주소를 사용했던 모든 위치를 찾아 변경해야 한다.

## Naming URL patterns
- URL에 이름을 지정하는 것 (path 함수의 name 인자를 정의해서 사용)

## URL 표기 변화

- naming하기 전
```html
<!-- articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>Hello, {{ name }}</h1>
  <a href="/dinner/">dinner</a>
  <a href="/search/">search</a>
  <a href="/throw/">throw</a>
{% endblock content %}
```
# :arrow_down:
- href 속성 값 뿐만 아니라 form의 action 속성처럼 url을 작성하는 모든 위치에서 변경
```html
<!-- articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>Hello, {{ name }}</h1>
  <a href="{% url 'dinner' %}">dinner</a>
  <a href="{% url 'search' %}">search</a>
  <a href="{% url 'throw' %}">throw</a>
{% endblock content %}
```

## 'url' tag
- 주어진 URL 패턴의 이름과 일치하는 절대 경로 주소를 반환
```python
{% url 'url-name' arg1 arg2 %}

>> variable routing의 경우에는 변수가 있기 때문에
>> 변수 인자를 공백을 기준으로 arg1 arg2 위치에 작성
```

# 5. URL Namespace

## URL 이름 지정 후 남은 문제
- articles 앱의 url 이름과 pages 앱의 url `이름이 같음`
- `단순히 이름만으로는 분리가 어려운 상황`
```python
# articles/urls.py

path('index/', views.index, name='index')
```
```python
# pages/urls.py

path('index/', views.index, name='index')
```

## :bulb: app_name 속성 지정

- url이름 _ app 이름표 붙이기
```python
# articles/urls.py

app_name = 'articles'
urlpatterns = [
  ...,
]
```
```python
# pages/urls.py

app_name = 'pages'
urlpatterns = [
  ...,
]
```

## URL tag의 변화
```python
{% url 'index' %}
```
## :arrow_down:
```python
{% url 'articles:index' %}
```

# 참고

## app_name 지정 후 주의사항
- app_name을 지정한 이후에는 url태그에서 반드시 app_name:url_name 형태로만 사용할 수 있음
- 그렇지 않으면 `NoReverseMatch 에러`가 발생
- 즉, app_name 지정 후 다음과 같은 표기는 사용 불가
```python
{% url 'index' %}
```

## Trailing Slashes
- django는 URL 끝에 '/'가 없다면 자동으로 붙임
- django의 url설계 철학
  - 기술적인 측면에서, foo.com/bar와 foo.com/bar/는 서로 다른 URL이다.
- 검색 엔진 롭봇이나 웹 트래픽 분석 도구에서는 이 두 주소를 서로 다른 페이지로 봄
- 그래서 django는 검색 엔진이 혼동하지 않게 하기 위해 사용
- 그러나 모든 프레임워크가 이렇게 동작하는 것은 아님