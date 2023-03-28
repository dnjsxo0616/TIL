# 1. ORM (Object-Relational-Mapping)
- 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 데이터를 변환하는 프로그래밍 기술

# 2. QuerySet API
- ORM에서 데이터를 검색, 필터링, 정렬 및 그룹화 하는 데 사용하는 도구
- (API를 사용하여 SQL이 아닌 Python 코드로 데이터를 처리)

## QuerySet API 구문
> Article.objects.all()
- Article : Model class
- objects : Manager
- all() : Queryset API

![Quertset_API](/image/Queryset_API.png)

## Query
- 데이터베이스에 특정한 데이터를 보여달라는 요청
- "쿼리문을 작성한다."  
  -> 원하는 데이터를 얻기 위해 데이터베이스에 요청을 보낼 코드를 작성한다.
- 이 때, 파이썬으로 작성한 코드가 ORM의 의해 SQL로 변환되어 데이터베이스에 전달되며, 데이터베이스의 응답 데이터를 ORM이 QuerySet이라는 자료 형태로 변환하여 우리에게 전달

## QuerySet
- 데이터베이스에게서 전달 받은 객체 목록(데이터 모음)
  - 순회가 가능한 데이터로써 1개 이상의 데이터를 불러와 사용할 수 있음
- Django ORM을 통해 만들어진 자료형
- 단, 데이터 베이스가 단일한 객체를 반환할 때는 QuerySet이 아닌 모델(Class)의 인스턴스로 반환됨

# 3. ORM CREATE

## QuerySet API 사전 준비

- 외부 라이브러리 설치 및 설정
```python
$ pip install ipython
$ pip install django-extensions
```

- settings.py에 등록을 해야 shell_puls이 사용가능하다.
- 일반 shell은 필요한 것들을 import해줘야하는 데
- shell_plus는 필요한 것들을 미리 import해준다.

```python
# settings.py

INSTALLED_APPS = [
  'articles',
  'django_extensions',
]
```

- pip로 뭔가 설치를 했으면 pip freeze > requirements.txt로 패키지 목록을 업데이트 해준다.

```python
$ pip freeze > requirements.txt
```

## Django Shell
- django 환경 안에서 실행되는 python shell
- (입력하는 QuerySet API 구문이 django 프로젝트에 영향을 미침)

## 데이터 객체를 만드는(생성하는) 3가지 방법
### - 첫 번째 방법 (1/3)
```python
# 특정 테이블에 새로운 행을 추가하여 데이터 추가

>>> article = Article() # Article(class)로부터 article(instance)
>>> article
<Article: Article object (None)>

>>> article.title = 'first' # 인스턴스 변수(title)에 값을 할당
>>> article.content = 'django!' # 인스턴스 변수(content)에 값을 할당

# save를 하지 않으면 아직 DB에 값이 저장되지 않는다.

>>> article
<Article: Article object (None)>

>>> Article.objects.all()
<QuertSet []>
```

### - 첫 번째 방법 (2/3)
- save 메서드를 호출해야 비로소 DB에 데이터가 저장(레코드 생성)
```python
# save를 하고 확인하면 저장된 것을 확인할 수 있다.

>>> article.save()
>>> article
<Article: Article object (1)>
>>> article.id
1
>>> article.pk
1
>>> Article.objects.all()
<QuerySet [Article: Article object (1)]>
```