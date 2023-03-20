# 1. Framwork
- 웹 애플리케이션을 빠르게 개발할 수 있도록 도와주는 도구
- (개발에 필요한 기본 구조, 규착, 라이브러리 등을 제공)

## 왜 프레임 워크를 사용할까:question:
- 기본적인 구조와 규칙을 제공하기 때문에 필수적인 개발에만 집중할 수 있다.
- 여러 라이브러리를 제공해 개발 속도를 빠르게 할 수 있다.
- 유지보수와 확장에 용이해 소프트웨어의 품질을 높인다.

# 2. django
- Python 기반의 대표적인 웹 프레임워크

# 3. 클라이언트와 서버

## 웹의 동작 방식
- 컴퓨터 혹은 모바일 기기로 웹 페이지를 볼 때 무슨 일이 발생할까?
- CLIENT ->(requests)-> SERVER
- CLIENT <- (responses) <- SERVER

## Client
- 서비스를 요청하는 주체 (웹 사용자의 인터넷이 연결된 장치, 웹 브라우저)

## Server
- 클라이언트의 요청에 응답하는 주체 (웹 페이지, 앱을 저장하는 컴퓨터)

## 우리가 웹 페이지를 보게되는 과정

1. 웹 브라우저`(클라이언트)`에서 'google.com'을 입력
2. 브라우저는 인터넷에 연결된 전세계 어딘 가에 있는 구글 컴퓨터`(서버)`에게 'Google 홈페이지.html' 파일을 달라고 요청
3. 요청을 받은 구글 컴퓨터는 데이터베이스에서 'Google 홈페이지.html' 파일을 찾아 우리 컴퓨터에게 응답
4. 전달받은 Google 홈페이지.html 파일을 웹 브라우저가 사람이 볼 수 있도록 해석해주면서 구글의 메인 페이지를 보게 된다.

## :bulb: django를 사용해서 서버(server)를 구현할 것

# 4. django 프로젝트 및 가상환경

## django 프로젝트 생성 전 루틴
```python
>> 1. 가상환경(venv) 생성
$ python -m venv venv # $ python -m venv [가상환경이름]

>> 2. 가상환경 활성화
$ source venv/Scripts/activate # $ source [가상환경이름]/Scripts/activate

>> 3. django 설치
$ pip install django==3.2.18 # pip install django만 작성하면 최신 버전으로 설치된다.

>> 4. 의존성 파일 생성 (패키지 설치시마다 진행)
$ pip freeze > requirements.txt

>> pip를 통해 현재 가상환경인지 확인
$ pip list
```

## django 프로젝트 생성
```python
$ django-admin startproject firstpjt .

>> fistpjt라는 이름의 프로젝트를 생성
```

## django 서버 실행
```python
$ python manage.py runserver
# python manage.py은 실제로 물리적인 파일이 있다. 그래서 python manage.py 이걸 기준으로 뒤쪽에 명령어를 작성한다.
```

## 서버 종료
```python
ctrl + c
```

## django 프로젝트 생성 루틴 정리

1. 가상환경 생성 (python -m venv venv)
2. 가상환경 활성화 (source venv/Scripts/activate)
3. django 설치 (pip install django==3.2.18)
4. 의존성 파일 생성 (패키지 설치시마다 진행) (pip freeze > requirements.txt)
5. (.gitignore 파일 생성)
6. (git 저장소 생성)
7. django 프로젝트 생성 (django-admin startproject firstpjt .)
8. 서버 실행 (python manage.py runserver)

- 5, 6번은 git 관련 설정 시에 진행한다.

## 페어의 입장
1. 환경에 대한 정보가 있는 파일을 clone으로 받았다.
2. 가상환경이란 것을 받은 것이 아니기 때문에 본인이 가상환경을 만들어야한다.
  - python -m venv venv
3. 가상환경을 ON 한다.
  - vscode의 방법 : Ctrl+shift+p -> interpreter -> python: Select Interpreter 선택 -> 지금 만든 가상환경을 클릭해준다. (ex. python 3.9.6 64-bit ('venv': venv))
  - 만약 터미널에 (venv)가 잡히지 않으면 터미널을 종료(휴지통) 후에 다시 터미널을 실행시킨다.
4. requirements.txt를 사용하여 패키지 목록 설치한다. (팀원과 동일한 환경 설치)
```python
$ pip install -r requirements.txt
>> requirements.txt에 있는 내용을 가지고 자동으로 패키지를 설치해줌으로써 
해당 프로젝트가 어떤 버전의 패키지를 썼는지 기억하지 않아도 개발환경을 설정 할 수 있다.
```
## :bulb: 환경 폴더 자체를 공유하지 않는다!! 요약본인 패키키 목록에 대한 버전과 이름이 적혀있는 텍스트 파일을 공유한다.

# 5. 참고

## 가상환경을 사용하는 이유

- 의존성 관리
  - 라이브러리 및 패키지를 각 프로젝트마다 독립적으로 사용 가능하다.
- 팀 프로젝트 협업
  - 모든 팀원이 동일한 환경과 의존성 위에서 작업하여 버전간 충돌을 방지한다.

## LTS(Long-Term Support)

- 프레임워크나 라이브러리 등의 소프트웨어에서 장기간 지원되는 안정적인 버전을 의미할 때 사용한다.
- 기업이나 대규모 프로젝트에서는 소프트웨어 업그레이드에 많은 비용과 시간이 필요하기 때문에 안정적이고 장기간 지원되는 버전이 필요하다.
