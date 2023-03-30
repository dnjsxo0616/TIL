# 1. HTTP request methods

- 게시글 작성 후 작성 완료를 나타내는 페이지를 렌더링 하는 것  
  -> 게시글을 `조회해줘!`라는 요청이 아닌 `작성해줘!`라는 요청이기 때문에 페이지 렌더링은 적절한 응답이 아니다.

## 데이터 저장 후 유저를 어디론가 다시 보내야 한다.

## redirect()
- 인자에 작성된 주소로 다시 요청을 보냄
- `redirect는 주소를 응답 / render는 템플릿을 응답`
- render를 통해 전체 목록 조회를 하려면 context로 데이터가 필요하고 주소도 이상해진다. ex. 생성할때쓰는 create주소가 전체 조회에 사용된다.
- redirect는 context없이 가능 
- `context의 역할`은 템플릿에 데이터와 함께 렌더링할 때 필요한 변수

```python
from django.shortcuts import render, redirect
from .models import Todo

def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')
    priority = request.POST.get('priority')
    deadline = request.POST.get('deadline')

    todo = Todo(title=title, content=content, priority=priority, deadline=deadline)
    todo.save()

    return redirect('todos:detail', todo.pk)
```
> redirect('todos:detail', todo.pk)
- create view 함수 수정
- redirect 함수 적용

## HTTP
- 네트워크 상에서 데이터를 주고 받기 위한 약속

## HTTP request methods
- 데이터(리소스)에 어떤 요청(행동)을 원하는지를 나타내는 것
- `GET` & `POST`

## 'GET' Method
- 특정 리소스를 조회하는 요청
- (GET으로 데이터를 전달하면 Query String 형식으로 보내짐)
- http://127.0.0.1:8000/articles/create/?`title=제목&content=내용`
- `반드시 데이터를 가져올 때만 사용해야한다.`

## 'POST' Method
- 특정 리소스에 변경사항을 만드는 요청
- (POST로 데이터를 전달하면 HTTP Body에 담겨 보내짐)

## POST method 적용
```python
<!-- templates/articles/new.html -->
<form action="{% url 'todos:create' %}" method="POST">
  {% csrf_token %}
  <div>
    <label for="title">제목</label>
    <input type="text" name='title' id="title">
  </div>
  <div>
    <label for="content">내용</label>
    <textarea name="content" id="content" cols="30" rows="10"></textarea>
  </div>
  <div>
    <label for="priority">우선순위</label>
    <input type="number" name="priority" id="priority">
  </div>
  <div>
    <label for="deadline">마감기한</label>
    <input type="date" name="deadline" id="deadline">
  </div>
  <input type="submit" value="할 일 생성">
</form>
```
```python
# todos/views.py

def create(request):
  title = request.POST.get('title')
  content = request.POST.get('content')
  priority = request.POST.get('priority')
  deadline = request.POST.get('deadline')
```

## HTTP response status code
- 특정 HTTP 요청이 성공적으로 완료되었는지 알려준다.
- 5개의 그룹으로 나뉘어짐(1xx, 2xx, 3xx, 4xx, 5xx)

## :bulb: "CSRF token이 누락되었으면"
```python
<!-- templates/articles/new.html -->
<form action="{% url 'todos:create' %}" method="POST">
  {% csrf_token %} # 이게 없다면
```

## 403 Forbidden
- 서버에 요청이 전달되었지만, 권한 때문에 거절되었다는 것을 의미

## CSRF(Cross-Site-Request-Forgery)
- "사이트 간 요청 위조"
- 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 `특정 웹 페이지를 보안에 취약하게 하거나 수정, 삭제 등의 작업`을 하게 만드는 공격 방법

## Security Token (CSRF Token)
- "대표적인 CSRF 방어 방법"
1. 서버는 사용자 입력 데이터에 임의의 난수 값(token)을 부여
2. 매 요청마다 해당 token을 포함시켜 전송시키도록 함
3. 이후 서버에서 요청을 받을 때마다 전달된 token이 유효한지 검증

## :star:DTL의 `csrf_token 태그`를 사용해 사용자에게 토큰 값을 부여 요청 시 토큰 값도 함께 서버로 전송될 수 있도록 함

## :star:POST Method는 데이터베이스에 대한 변경사항을 만드는 요청이기 때문에 토큰을 사용해 최소한의 신원 확인을 하는 것

# 2. DELETE

```python
# todos/urls.py
from django.urls import path
from . import views

app_name="todos"
urlpatterns = [
  ...
  path('<int:todo_pk>/delete/', views.delete, name="delete"),
]

# todos.view.py
from django.shortcuts import render, redirect
from .models import Todo

def delete(request, todo_pk):
    todo = Todo.objects.get(pk=todo_pk)
    todo.delete()
    return redirect('todos:index')
```
```html
<!-- articles/detail.html -->
  <p>번호 - {{todo.pk}}</p>
  ...
  <form action="{% url 'todos:delete' todo.pk%}" method="POST">
    {% csrf_token %}
    <input type="submit" value="DELETE">
  </form>
  <a href="{% url 'todos:index' %}">[back]</a>
```
# 3. Update

## Update 로직을 구현하기 위해 필요한 view 함수
- `edit` : 사용자의 입력을 받는 페이지를 렌더링
- 'update' : 사용자가 입력한 데이터를 받아 DB에 저장


## edit 로직 작성
```python
# todos/urls.py
from django.urls import path
from . import views

app_name="todos"
urlpatterns = [
  ...
  path('<int:todo_pk>/edit/', views.edit, name="edit"),
]

# todos.view.py
from django.shortcuts import render, redirect
from .models import Todo

def edit(request, todo_pk):
    todo = Todo.objects.get(pk=todo_pk)
    context = {
        'todo':todo,
    }
    return render(request, 'articles/edit.html', context)
```
```html
<!-- articles/edit.html -->

<form action="{% url 'todos:update' todo.pk%}" method="POST">
  {% csrf_token %}
  <div>
    <label for="title">제목</label>
    <input type="text" name='title' id="title" value={{todo.title}}>
  </div>
  <div>
    <label for="content">내용</label>
    <textarea name="content" id="content" cols="30" rows="10">{{todo.content}}</textarea>
  </div>
  <div>
    <label for="priority">우선순위</label>
    <input type="number" name="priority" id="priority" value={{todo.priority}}>
  </div>
  <div>
    <label for="deadline">마감기한</label>
    <input type="date" name="deadline" id="deadline" value={{todo.deadline}}>
  </div>
  <input type="submit">
</form>
<hr>
<a href="{% url 'todos:index' %}">[back]</a>
```

## edit 페이지로 이동하기 위한 하이퍼링크 작성

```html
<!-- articles/detail.html-->

<p>번호 - {{todo.pk}}</p>
<p>제목 - {{todo.title}}</p>
<p>내용 - {{todo.content}}</p>
<p>우선순위 - {{todo.priority}}</p>
<p>마감기한 - {{todo.deadline}}</p>
<p>완료여부 - {{todo.completed}}</p>
<form action="{% url 'todos:delete' todo.pk%}" method="POST">
  {% csrf_token %}
  <input type="submit" value="DELETE">
</form>
<a href="{% url 'todos:index' %}">[back]</a>
<a href="{% url 'todos:edit' todo.pk %}">[수정]</a>
```
- 링크
```html
<form action="{% url 'todos:delete' todo.pk%}" method="POST">
```

## update 로직 작성

```python
# todos/urls.py
from django.urls import path
from . import views

app_name="todos"
urlpatterns = [
  ...
  path('<int:todo_pk>/update/', views.update, name="update"),
]

# todos.view.py
from django.shortcuts import render, redirect
from .models import Todo

def update(request, todo_pk):
  todo = Todo.objects.get(pk=todo_pk)
  todo.title = request.POST.get('title')
  todo.content = request.POST.get('content')
  todo.priority = request.POST.get('priority')
  todo.deadline = request.POST.get('deadline')
  todo.save()
  return redirect('todos:detail', todo.pk)
```
```html
<form action="{% url 'todos:update' todo.pk%}" method="POST">
  {% csrf_token %}
  <div>
    <label for="title">제목</label>
    <input type="text" name='title' id="title" value={{ todo.title }}>
  </div>
  <div>
    <label for="content">내용</label>
    <textarea name="content" id="content" cols="30" rows="10">{{todo.content}}</textarea>
  </div>
  <div>
    <label for="priority">우선순위</label>
    <input type="number" name="priority" id="priority" value={{ todo.priority}}>
  </div>
  <div>
    <label for="deadline">마감기한</label>
    <input type="date" name="deadline" id="deadline" value={{todo.deadline}}>
  </div>
  <input type="submit">
</form>
<hr>
<a href="{% url 'todos:index' %}">[back]</a>
```
- edit.html에 update url 연결
```html
<form action="{% url 'todos:update' todo.pk%}" method="POST">
```

# 참고
## (POST)  articles/1/ 1번 게시글 생성할거야!!
## (DELETE)  articles/1/ 1번 게시글 삭제할거야!!
