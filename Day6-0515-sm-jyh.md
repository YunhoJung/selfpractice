#Day6
-

##vim
- 리눅스 환경에서 많이 사용하던 문서편집기
- vim ~/.vimrc (vim 환경설정 들어갈 때)

> .은 숨김파일

##git : 프로그램 등의 소스 코드 관리를 위한 분산 버전 관리 시스템
- 버전 관리 시스템이란 파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시 꺼내올 수 있는 시스템을 말한다.
- git의 장점 : 이미 진행된 프로젝트를 통째로 이전 상태로 되돌릴 수 있고 시간에 따라 수정 내용을 비교해 볼 수 있고 누가 언제 만들어낸 이슈인지도 추적할 수 있다. 그리고 잘못되었을 때도 쉽게 복구할 수 있다. 내 컴퓨터의 로컬 저장소에 작업 내용을 커밋하고 다른 사람과 공유할 때 원격저장소로 푸쉬한다. 따라서 네트워크가 불가한 상황에서도 작업을 계속할 수 있다.

- git의 무결성 : 데이터를 저장하기 전에 항상 체크섬을 구하고 그 체크섬으로 데이터를 관리.
- 해시 함수 : 해시 함수의 질은 해시 충돌률로 결정. 해시를 사용해서 체크섬을 만든다.
- sha : 해시 알고리즘. 암호학적 해시 함수들의 모음. (cf.md5)
- cli : 명령줄 인터페이스

- gui : 그래픽 유저 인터페이스 (제록스 파크)
> ex) 윈도우

####git 구조
- committed이란 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미
- Modified는 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 의미
- Staged란 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태

1. 워킹 디렉토리에서 파일을 수정한다.
2. staging Area에 파일을 Stage 해서 커밋할 스냅샷을 만든다.
3. Staging Area에 있는 파일들을 커밋해서 Git 디렉토리에 영구적인 스냅샷을 저장한다.

####git 명령어
 
```
$ git config (git 설정 내용 확인하고 변경하는 명령어)
$ git config --list (config로 설정한 리스트 확인)
$ git --version (버전 확인)
$ git help tag (도움말)
$ git config --global user.name "자기 이름" (사용자 정보 중 이름 설정)
$ git config --global user.email 이메일 (사용자 정보 중 이메일 설정)
$ git init(git 환경 세팅, 초기화)
$ git add 파일명.md(마크다운의 경우) staged 상태에 넣기(커밋은 안되었지만 커밋 준비가 된)
$ git commit(첫 번째 줄에 한 줄 입력해야 커밋됨)
$ git commit -m 'add 추가 수정' ( add 추가 수정이라는 문구를 입력한 뒤 커밋)
$ git status(현재 워킹디렉토리 상태 확인)
$ git log(커밋된 내용확인)
$ git add --all(모든 파일을 staged 상태로 추가)
$ git diff --staged(staged된 것만 수정된 사항 표시)
$ git diff (modified 영역에서 뭐가 달라졌나)
$ echo 'show me the money' > abc4.txt : 'show me the money' 내용의 abc4.txt 생성
$ cat abc4.txt : abc4.txt 내용 표시
$ rm -rf .git(깃 해체)
```


- untracked file 은 커밋이 되지 않는다.

- 파일 무시하기
.gitignore 파일을 만들고 그 안에 무시할 파일 패턴을 적는다.
커밋이랑은 관계없이 파일을 만들자마자 효력 발생.(* : 문자 전체 의미)

> ex) *.swp(임시파일)

##zsh
zsh :
> 확인법 : echo $SHELL

vi ~/.zshrc
-명령어 설정하기
> alias <사용할 명령어> = "<명령어 내용>"

```
$ alias atom="open -a /Applications/Atom.app/Contents/MacOS/Atom"
$ alias md="open -a /Applications/MacDown.app/Contents/MacOS/MacDown"
$ alias py="open -a /Applications/Pycharm\ CE.app/Contents/MacOS/pycharm"
```











<!--
커밋하려는 스테이지 상태에 있는 파일과 그것을 다시 수정한 modified된 파일은 따로. 두개로 나누냐 하나냐의 차이. 따로 해주기 각 각 해주기

-->

<!--ls -s ~/Dropbox/zsh/.zshrc ~/.zshrc        
-cd -/Dropbox/zsh

$git commit -a -m 'added new benchmarks'
git rm과 rm 의 차이
```
zsh l
zsh git init
git add .zshrc
git status
git commit
~/Dropbox/zsh git status
git --git-dir ~/Dropbox/zsh/.git log
```

untracked와 modified
스테이지영역이란 커밋하기 위해서 중간에 커밋하기 위한 파일의 집합
스테이지영역이 필요한 이유는 커밋하려는 파일 나누기 위해
$git log -p
-숫자하면 최근 숫자만큼의 수정사항 보여주및
git clone??
git add -> git commit -> git push
git fetch -> git merge (이 둘을 합치면 git pull)-->