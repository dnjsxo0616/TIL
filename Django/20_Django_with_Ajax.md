# 1. 개요

## 1-1 비동기 (Asynchronous)
- 작업을 시작한 후 결과를 기다리지 않고 다음 작업을 처리하는 것 (병렬적 수행)
- 시간이 필요한 작업들은 요청을 보낸 뒤 응답이 빨리 오는 작업부터 처리
- 예시
  - Gmail에서 메일 전송을 누르면 목록 화면으로 전환되지만 실제로 메일을 보내는 작업은 병렬적으로 뒤에서 처리됨

## 1-2 비동기 예시

![비동기예시](./image/%EB%B9%84%EB%8F%99%EA%B8%B01.png)

## 1-3 Ajax (Aynchronous JavaScript And XML(JSON))
- 비동기적인 웹 애플리케이션 개발을 위한 프로그래밍 기술명
- ` 사용자의 요청에 대한 즉각적인 반응을 제공하면서,`
- `페이지 전체를 다시 로드하지 않고 필요한 부분만 업데이트 하는 것을 목표`

## 1-4 응답의 변화

![응답의 변화1](./image/%EC%9D%91%EB%8B%B5%EB%B3%80%ED%99%941.png)

![응답의 변화2](./image/%EC%9D%91%EB%8B%B5%EB%B3%80%ED%99%942.png)

- `좋아요 취소` 요청을하면 -> `django`는 좋아요를 취소한 결과를 그려서 보내주는 것이 아니라 좋아요 취소한 사실이 `담긴 데이터를 주고` -> 데이터를 `JavaScript`에서 `페이지를 다시 그린다.`(JS는 문서 조작이 가능하기 때문이다.) 
- 문서를 매 번 새로운 주소로 받는 것이 아니라 문서를 그 위치에서 다시 그린다.(문서가 새로고침이 안됨)

## 1-5 XMLHttpRuquest
- JavaScript 객체로, 클라이언트와 서버 간에 데이터를 비동기적으로 주고 받을 수 있도록 해주는 객체
- `JavaScript 코드에서 서버에 요청을 보내고, 서버로부터 응답을 받을 수 있음`

# 2. 비동기 요청

## 2-1 Axios
- JavaSript에서 HTTP 요청을 보내는 라이브러리 주로 프론트엔드 프레임워크에서 사용

## 2-2 Axios 기본 문범
- get, post 등 여러 method 사용가능
- then을 이용해서 성공하면 수행할 로직을 작성
- catch를 이용해서 실패하면 수행할 로직을 작성

![Axios기본문법](./image/axios%EA%B8%B0%EB%B3%B8%EB%AC%B8%EB%B2%95.png)

## 2-3 Axios 예시

### 2-3-1 고양이 사진 가져오기 (Python)

![axiospython](./image/%EA%B3%A0%EC%96%91%EC%9D%B4%EC%82%AC%EC%A7%84(python).png)

### 2-3-2 고양이 사진 가져오기 (JavaScript)

![axiosjs](./image/%EA%B3%A0%EC%96%91%EC%9D%B4%EC%82%AC%EC%A7%84(js).png)

### 2-3-3 고양이 사진 가져오기 (결과 비교)
- 동기식 코드 (python)는 위에서부터 순서대로 처리가 되기 때문에 첫 번째 print가 출력되고 이미지를 가져오는 처리를 기다렸다가 다음 print가 출력되는 반면
- 비동기식 코드 (JavaSript)는 바로 처리가 가능한 작업(console.log)은 바로 처리하고, 오래 걸리는 작업인 이미지를 요청하고 가져오는 일은 요청을 보내 놓고 기다리지 않고 다음 코드로 진행 후 완료가 된 시점에 결과 출력이 진행된다.

### 2-3-4 고양이 사진 가져오기 (완성하기)

![고양이사진완성](./image/%EA%B3%A0%EC%96%91%EC%9D%B4%EC%82%AC%EC%A7%84(%EC%99%84%EC%84%B1).png)

# 3. 팔로우 with ajax

## 3-1 팔로우 구현
## 3-1-1 axios CDN 작성
```html
<!-- accounts/profile.html -->

  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  <script>
  </script>
</body>
</html>
```

## 3-1-2 form 요소 선택을 위해 id 속성 지정 및 선택
- 불필요해진 action과 method 속성은 삭제 (요청은 axios로 대체되기 때문)
```html
<!-- acccounts/profile.html -->

<form id="follow-form">
  ...
</form>

<script>
  const form = document.querySelector('#follow-form')
</script>
```

## 3-1-3 axios 요청 준비
```html
<!-- accounts/profile.html -->

<script>
  const form = document.querySelector('#follow-form')
  form.addEventListener('submit', function (event) {
    event.preventDefault()
    axios({
      method: 'post',
      url: '/accounts/${???}/follow/',
    })
  })
</script>
```

## 3-1-4 현재 axios로 POST 요청을 보내기 위해 필요한 것
1. url에 작성할 user pk는 어떻게 작성해야 할까?
2. csrftoken은 어떻게 보내야 할까?

## 3-1-5 url에 작성할 user pk 가져오기 (HTML -> JavaScript)

![팔로우구현](./image/)

## 'data-*' attributes

- 사용자 지정 데이터 특성을 만들어 임의의 데이터를 HTML과 DOM사이에서 교환할 수 있는 방법
```html
<div data-my-id="my-data"></div>

<script>
  const myId = event.target.dataset.myId
</script>
```

- 예를 들어 data-test-value라는 이름의 특성을 지정했다면 JavaScript에서는 element.dataset.testValue로 접근할 수 있음

## 3-1-6 요청 url 작성 마치기

![팔로우구현](./image/%ED%8C%94%EB%A1%9C%EC%9A%B0%EA%B5%AC%ED%98%842.png)

## 3-1-7 먼저 hidden 타입으로 숨어있는 csrf 값을 가진 input 태그를 선택해야 한다.

![팔로우구현](./image/%ED%8C%94%EB%A1%9C%EC%9A%B0%EA%B5%AC%ED%98%843.png)

![팔로우구현](./image/%ED%8C%94%EB%A1%9C%EC%9A%B0%EA%B5%AC%ED%98%844.png)

## 3-1-8 AJAX로 csrftoken을 보내느 방법

![팔로우구현](./image/%ED%8C%94%EB%A1%9C%EC%9A%B0%EA%B5%AC%ED%98%845.png)

## 3-1-9 팔로우 구현
- 팔로우 버튼을 토글하기 위해서는 현재 팔로우가 된 상태인지 여부 확인이 필요
- axios 요청을 통해 response 객체를 활용해 view 함수를 통해서
- 팔로우 관계 여부를 파악할 수 있는 변수를 담아 JSON 타입으로 응답하기

## 3-1-10 팔로우 관계를 확인하기 위한 is_followed 변수 작성 및 JSON 응답

![팔로우구현6](./image/%ED%8C%94%EB%A1%9C%EC%9A%B0%EA%B5%AC%ED%98%846.png)

## 3-1-11 view 함수에서 응답한 is_followed를 사용해 버튼 토글하기

![팔로우구현7](./image/%ED%8C%94%EB%A1%9C%EC%9A%B0%EA%B5%AC%ED%98%847.png)

## 3-1-12 결과 확인 (개발자 도구 - Network)

![팔로우구현8](./image/%ED%8C%94%EB%A1%9C%EC%9A%B0%EA%B5%AC%ED%98%848.png)

## 3-2-1 팔로잉 & 팔로워 수 비동기 적용
- 해당 요소를 선택할 수 있도록 span 태그와 id 속성 작성
```html
<!-- accounts/profile.html -->

<div>
  팔로잉 : <span id="followings-count">{{ person.followings.all|length }}</span>
  팔로워 : <span id="followers-count">{{ person.followers.all|length }}</span>
</div>
```

## 3-2-2 직전에 작성한 span 태그를 각각 선택

![팔로잉팔로워1](./image/%ED%8C%94%EB%A1%9C%EC%9E%89%ED%8C%94%EB%A1%9C%EC%9B%8C1.png)

## 3-2-3 팔로잉, 팔로워 인원 수 연산은 view 함수에서 진행하여 결과를 응답으로 전달

![팔로잉팔로워2](./image/%ED%8C%94%EB%A1%9C%EC%9E%89%ED%8C%94%EB%A1%9C%EC%9B%8C2.png)

## 3-2-4 view 함수에서 계산된 결과를 응답에서 찾아 적용

![팔로잉팔로워3](./image/%ED%8C%94%EB%A1%9C%EC%9E%89%ED%8C%94%EB%A1%9C%EC%9B%8C3.png)

## 3-3-1 최종 코드 - HTML

![최종코드1](./image/%EC%B5%9C%EC%A2%85%EC%BD%94%EB%93%9C1.png)

## 3-3-2 최종 코드 - View

![최종코드2](./image/%EC%B5%9C%EC%A2%85%EC%BD%94%EB%93%9C2.png)

## 3-3-3 최종 코드 -JS

![최종코드3](./image/%EC%B5%9C%EC%A2%85%EC%BD%94%EB%93%9C3.png)

# 4. 좋아요 with ajax

## 4-1 좋아요 비동기 적용
- 팔로우와 동일한 흐름 + forEach() + querySelectorAll()
  - index 페이지 각 게시글에 각각 좋아요 버튼이 있기 때문
  - 반복 + 목록 선택

## 4-2-1 최종 코드 - HTML

![최종코드](./image/%EC%A2%8B%EC%95%84%EC%9A%94%EC%B5%9C%EC%A2%85%EC%BD%94%EB%93%9C1.png)

## 4-2-2 최종 코드 - View

![최종코드](./image/%EC%A2%8B%EC%95%84%EC%9A%94%EC%B5%9C%EC%A2%85%EC%BD%94%EB%93%9C2.png)

## 4-2-3 최종 코드 - JS

![최종코드](./image/%EC%A2%8B%EC%95%84%EC%9A%94%EC%B5%9C%EC%A2%85%EC%BD%94%EB%93%9C3.png)

# 참고

## 비동기를 사용하는 이유
### "사용자 경험"
- 예를 들어 아주 큰 데이터를 불러온 뒤 실행되는 앱이 있을 때, 동기로 처리한다면 데이터를 모두 불러온 뒤에야 앱의 실행 로직이 수행되므로 사용자들은 마치 앱이 멈춘 것과 같은 경험을 겪에 됨
- 즉, 동기식 처리는 특정 로직이 실행되는 동안 다른 로직 실행을 차단하기 때문에 마치 프로그램이 응답하지 않는 듯한 사용자 경험을 만들게 됨
- 비동기로 처리한다면 먼저 처리되는 부분부터 보여줄 수 있으므로, 사용자 경험에 긍정적인 효과를 볼 수 있음
- 이와 같은 이유로 많은 웹 기능은 비동기 로직을 사용해서 구현되어 있음