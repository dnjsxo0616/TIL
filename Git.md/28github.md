# Github로
- add commit 후 버전을 올리는 것이 Github다.

## push

**제일 먼저** 저장도 중요!

```bush
$ git init
```

### (1) 로컬 저장소에 원격 저장소 정보 설정하기
- 로컬 저장소에는 한번만 설정하면 된다.
```bush
$ git remote add origin <URL>
```

### (2) 로컬 저장소의 버전을 원격 저장소로 `push`하기
- $ git push <원격저장소이름><브랜치이름>
  원격 저장소는 로컬 폴더의 파일/폴더가 아닌 저장소의 `버전(커밋)`을 관리하는 것

#### 1. 작성한 파일 add
```bush
$ git add 28git.md
```
#### 2. commit
```bush
$ git commit -m '28git'
[master 50f9627] 28git
 1 file changed, 72 insertions(+)
 create mode 100644 28git.md
```
#### 3. push
```bush
$ git push origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.12 KiB | 67.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/dnjsxo0616/TIL.git
   403c7fb..50f9627  master -> master
```
- 추가적인 변경사항은 add -> commit -> push
---
## pull

### 원격 저장소의 버전을 로컬 저장소로 `pull`하기
- $ git pull <원격저장소이름><브랜치이름>
 원격 저장소로부터 변경된 내역을 받아와서 이력을 병합함
---
## clone
- 저장소 자체를 가져오는 행위이다.

### 원격저장소 프로젝트를 시작하고 싶을 때 `clone`
- & git clone <원격저장소 주소>
 원격 저장소를 복제하여 가져옴
```bush
$ git clone https://github.com/kdt-live/TIL-2nd.git
Cloning into 'TIL-2nd'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.      
remote: Compressing objects: 100% (2/2), done.   
remote: Total 4 (delta 0), reused 4 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
```
---
## gitignore
- 버전관리랑 상관없는 파일을 관리하는 법
```
.gitignore
```
- Git 저장소에 .gitignore파일을 생성한 후 상관없는 파일을 관리한다.
- 확장자가 같은 파일이 많으면 ex) pptx 라면 *.pptx라고 하면 모든 pptx를 무시 가능

  **주의** 이미 커밋된 파일은 반드시 삭제를 해야지 적용된다.
---
- `clone`과 `pull`의 차이점은?
clone : 원격저장소 복제
pull : 원격저장소 커밋 가져오기

Git log를 했을 때 : 은 길어서 그런 건데 q(quit)를 누르면 된다.

## 명령어 정리
```bush
$ git clone <url> : 원격 저장소 복제
$ git remote -v : 원격 저장소 정보 확인
$ git remote add <원격저장소> <url> : 원격 저장소 추가(일반적으로 origin)
$ git remote re <원격저장소> : 원격저장소 삭제
$ git push <원격저장소> <브랜치> : 원격저장소에 push
$ git pull <원격저장소> <브랜치> : 원격저장소로부터 pull
```

