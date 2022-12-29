# git
버전 관리

## 버전을 기록하는 방법
  1. 작업하면
  2. add하여 staging area에 모아
  3. commit으로 버전 기록한다.

## git 기초 명령어
  - git init : 로컬 저장소 생성
  - git add 파일명 : 특정 파일/폴더의 병경사항 추가
  - git commit -m '커밋메세지' : 커밋(버전기록)
  - git status : 상태 확인
  - git log : 버전 확인

### $ git add <file>
- untracked 상태의 파일을 staged로 변경
- modified 상태의 파일을 staged로 변경

### $ git commit -m '커밋메세지'
- staged 상태의 파일들을 커밋을 통해 버전으로 기록

### `기본 흐름`
- git 파일을 modified, staged, committed로 관리
  - modified : 파일이 수정된 상태(add 명령어를 통하여 staging area로)
  - staged : 수정한 파일을 곧 커밋할 것이라고 표시한 상태 (commit 명령어로 저장소)
  - committed 커밋이 된 상태