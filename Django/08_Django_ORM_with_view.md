# 1. READ

## 전체 게시글 조회
```python
# articles/views.py

from .models import Article

def index(request):
  articles = Article.objects.all()
  context = {
    'articles': articles,
  }
  return render(request, 'articles/index.html', context)
```
```html
<!--articles/index.html-->

<h1>Articles</h1>
<hr>
{% for article in articles %}
  <p>글 번호: {{ article.pk }}</p>
  <p>글 제목: {{ article.title }}</p>
  <p>글 내용: {{ article.content }}</p>
  <hr>
{% endfor %}
```

## 단일 게시글 조회
```python
# articles/urls.py

app_name = 'articles'
urlpatterns = [
  ...
  path('<int:pk>/', views.detail, name = 'detail'),
]
```
```python
# articles/views.py

def detail(request, pk):
  article = Article.objects.get(pk=pk)
  context = {
    'article': article,
  }
  return render(request, 'articles/detail.html', context)
```
```html
<!-- templates/articles/detail.html -->

<h2>DETAIL</h2>
<h3>{{ article.pk }} 번째 글</h3>
<hr>
<p>제목: {{ article.title }}</p>
<p>내용: {{ article.content }}</p>
<p>작성 시각: {{ article.created_at }}</p>
<p>수정 시각: {{article.updated_at }}</p>
<hr>
<a href="{% url 'articles:index' %}">[back]</a>
```

## 제목을 누르면 해당 글의 상세 페이지로 이동
```html
<!-- templates/articles/detail.html -->

<h1>Articles</h1>
<hr>
{% for article in articles %}
  <p>글 번호: {{ article.pk }}</p>
  <a href="{% url 'articles:detail' article.pk %}">
    <p>글 제목: {{ article.title }}</p>
  </a>
  <p>글 내용: {{ article.content }}</p>
  <hr>
{% endfor %}
```

# 2. CREATE

## Create 로직을 구현하기 위해 필요한 view 함수
- 사용자의 입력을 받는 페이지를 렌더링 -> `new`
- 사용자가 입력한 데이터를 받아 DB에 저장 -> `create`