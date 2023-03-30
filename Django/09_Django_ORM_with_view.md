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