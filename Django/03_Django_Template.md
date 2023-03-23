# 1. Template System

## django template system
- 데이터 `표현`을 제어하면서, `표현`과 관련된 로직을 담당

## HTML의 특정 부분을 변수 값에 따라 바꾸고 싶다면:question:
- views.py에서 조건을 주고 templates의 html에서 출력하면 된다.
```python
>>> views.py
def today_dinner(request):
    foods = ['볶음밥', '보쌈', '서브웨이', '햄버거',]
    food = random.choice(foods)
    context = {
        'foods' : food,
    }
    return render(request, 'articles/today_dinner.html', context)
```
```html
>>> today_dinner.html
<body>
  <h1>오늘의 추천 저녁 메뉴는 <span>{{ foods }}</span>입니다.</h1>
```

## Django Template Language (DTL)
- Template에서 조건, 반복, 변수, 필터 등의 프로그래밍적 기능을 제공하는 시스템

## DTL 구조
1. Variable
2. Filters
3. Tags
4. Comments

## Variable
- view 함수에서 render 함수의 세 번째 인자로 딕셔너리 타입으로 넘겨 받을 수 있음
- 딕셔너리 Key에 해당하는 문자열이 template에서 사용 가능한 변수명이 됨
- dot(.)를 사용하여 변수 속성에 접근할 수 있음
```python
{{ variable }}
```

## Filters
- 표시할 변수를 수정할 때 사용
- chained가 가능하며 일부 필터는 인자를 받기도 한다
  ```python
  {{ namd | truncatewords:30 }}
- 약 60개의 built-in template filters를 제공
  ```python
  {{ variable | filter }}

## Tags
- 반복 또는 논리를 수행하여 제어 흐름을 만드는 등 변수보다 복잡한 일들을 수행
- 일부 태그는 시작과 종료 태그가 필요
  ```python
  {% if %} {% endif %}
- 약 24개의 built-in template tags를 제공
  - {% tag %}

# 2. 템플릿 상속 (Template inheritance)
- 페이지의 공통요소를 포함하고, 하위 템플릿이 재정의 할 수 있는 공간을 정의하는 기본 'skeleton' 템플릿을 작성하여 상속 구조를 구축

## skeleton 역할 템플릿 작성
```html
>> articles/base.html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Document</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
    {% block style %}
    {% endblock style %}
  </head>
  <body>
    {% block content %}
    {% endblock content %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
  </body>
</html>
```
## 기존 템플릿의 변화
```html
{% extends 'articles/base.html' %}

{% block style %}
  <style>
    span {
      color: crimson;
    }
  
    h1 {
      text-align: center;
    }
  </style>
{% endblock style %}

{% block content %}
  <h1>오늘의 추천 저녁 메뉴는 <span>{{ foods }}</span>입니다.</h1>
{% endblock content %}
```

## 'extends' tag
```python
{% extends 'path' %}

>>'path'에 상속받아올 부모html을 설정한다 ex. 'articles/base.html'
```
- 자식(하위) 템플릿이 부모 템플릿을 확장한다는 것을 알림
- `반드시 템플릿 최상단에 작성되어야 한다. (2개 이상 사용 불가)`

## 'block' tag
```python
{% block name %}{%endblock name %}
```
- 하위 템플릿에서 재정의(overridden)할 수 있는 블록을 정의 (하위 템플릿이 작성할 수 있는 공간을 지정)

## 상속을 하는 이유
- `재사용성` 때문이다. ex. 부트스트랩을 모든 문서에 적용시킬 때

# 3. 요청과 응답 with HTML form

## 데이터를 보내고 가져오기 (sending and Retrieving form data)
- HTML form element를 통해 사용자와 애플리케이션 간의 상호작용 이해하기
- HTML form은 HTTP 요청을 서버에 보내는 가장 편리한 방법이다.

## 예시
- form만으로는 아무것도 할 수가 없다.
- input은 form에 종속된다. input type으로 여러가지 설정이 가능하다.

```python
{% block content %}
  <h1>Form 실습</h1>
  <form action="">
    <input type="text">
    <input type="submit">
  </form>
{% endblock content %}

>> input에서 입력받은 값이 action의 주소로 보내진다.
```
- django를 입력하게 되면 message=Django의 key = value 형태로 서버에 전송이 된다.
- http://127.0.0.1:8000/search/?message=django 주소에서도 나타난다.
- 서버는 django라는 값이 필요한데 서버에서 접근할 때 쓰는 값은 message이름으로 접근해서 사용자 입력값(Django)를 사용한다.
- 검색어를 입력하면 서버에서는 url로 소통했던 것이다. 입력 받고 url로 값을 찾아서 페이지를 만들고 사용자에게 반환한다. http://127.0.0.1:8000/search/?message=django 이런 식으로
- ?message=Django 이 부분을 query string parameters라고 부른다.
```python
{% extends 'articles/base.html' %}

{% block content %}
  <h1>Form 실습</h1>
  <form action="">
    <input type="text" name="message">
    <input type="submit">
  </form>
{% endblock content %}
```

## 'form' element
- 사용자로부터 할당된 데이터를 서버로 전송, 웹에서 사용자 정보를 입력하는 여러 방식(text, password 등)을 제공한다.

## 'action' & 'method'
- form의 핵심 속성 2가지
- 데이터를 `어디(action)`로 어떤 `방식(method)`으로 보낼지
- form의 method의 기본 값은 GET이다. method = 'GET'

## action과 method
- action
  - 입력 데이터가 전송될 URL을 지정 (목적지)
  - 만약 이 속성을 지정하지 않으면 데이터는 현재 form이 있는 페이지의 URL로 보내짐
- method
  - 데이터를 어떤 방식으로 보낼 것인지 정의
  - 데이터의 HTTP request methods (GET, POST)를 지정

## 'input' element
- 사용자의 데이터를 입력 받을 수 있는 요소 (type 속성 값에 따라 다양한 유형의 입력 데이터를 받는다.)

## 'name' (input의 핵심 속성)
- 데이터를 제출했을 때 서버는 name 속성에 설정된 값을 통해 사용자가 입력한 데이터에 접근할 수 있다.

## label 태그
- 의미론적인 태그(Semantic)
- input앞에 검색어 : 가 붙도록한다. for가 input의 id값과 일치시킨다 그래서 검색어를 클릭하면 어디서 입력을해야하는지 알려준다 input박스가 클릭되는 효과
```html
{% block content %}
  <h1>Form 실습</h1>
  <form action="https://search.naver.com/search.naver" method='GET'>
    {% comment %} 목적지url {% endcomment %}
    <label for="message">검색어 : </label>
    <input type="text" name="query" id="message">
    <input type="submit">
  </form>
{% endblock content %}
```

## Query String Parameters
- 사용자의 입력 데이터를 URL 주소에 파라미터를 통해 넘기는 방법
- 문자열은 앰퍼샌드(&)로 연결된 Key=value 쌍으로 구성되며, 기본 URL과 물음표(?)로 구분됨
- 예시
  - http://host:port/path?`key=value`&`keyvalue`

# 4. 요청과 응답 활용
- 사용자 입력 데이터를 받아 그대로 출력하는 서비스 제작

## form 데이터는 어디에 들어 있을까:question:
- 모든 요청 데이터는 `HTTP request` 객체에 들어있다. (view 함수의 첫번째 인자인 request)

# 참고

## 추가 템플릿 경로 지정
- settings.py 파일에 TEMPLATES = {}에서 'DIRS에 'DIRS' : [BASE_DIR / 'templates',],로 작성
- {% extends 'base.html' %} -> extends 경로 수정

## Base_DIR
- settings에서 경로지정을 편하게 하기 위해 최상단 지점을 지정해놓은 변수

## DTL 주의사항
- Python처럼 일부 프로그래밍 구조(if, for 등)를 사용할 수 있지만 명칭을 그렇게 설계했을 뿐이지 python코드로 실행되는 것이 아니며 python과 아무런 관련이 없음
- 프로그래밍적 로직이 아니라 프레젠테이션을 표현하기 위한 것임을 명심
  - 포그래밍적 로직은 되도록 view함수에서 작성 및 처리