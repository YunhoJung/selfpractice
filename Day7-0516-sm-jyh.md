#Day7

##git 되돌리기


git 에서는 가장 최신버전이 원본. 델타는 되돌아갈 때, 왜냐하면 가장 최신버전일 수록 불러올 가능성이 높기 때문이다.
작업을 하다보면 모든 단계에서 어떤 것은 되돌리고 싶을 때가 있다. git을 사용하면 작업을 하다가 되돌릴 수 있다. 하지만 한 번 되돌리면 복구할 수 없기 때문에 주의해야 한다.

종종 완료한 커밋을 수정해야 할 때가 있다. 너무 일찍 커밋했거나 어떤 파일을 빼먹었을 때, 그리고 커밋 메시지를 잘못 적었을 때 한다. 다시 커밋하고 싶으면 '--amend' 옵션을 사용한다.

```
$ git add --amend
```

> 이 명령은 staging area를 사용하여 커밋한다. 만약 마지막으로 커밋하고 나서 수정한 것이 없다면(커밋하자마자 바로 이 명령을 실행하는 경우) 조금 전에 한 커밋과 모든 것이 같다. 이 때는 커밋 메시지만 수정한다. 

편집기가 실행되면 이전 커밋 메시지가 자동으로 포함된다. 메시지를 수정하지 않고 그대로 커밋해도 기존의 커밋을 덮어쓴다.

<!--$ git log -p-->

```
$ git commit -a -m "" : 커밋하면서 메시지도 같이
```

> tracked되고 있는 파일만 추가 한번도 깃에서 관리되지 않은 파일은 안됨

```
$ git commit --amend
```

> 이건 커밋이 지워지고 새로 같이 덮어쓰는 것
두 번째 커밋은 첫 번째 커밋을 덮어쓴다.

<!--staged area에서 따로 빼내고 싶을때-->


**모든 파일 git에 추가하기**
```
git -A
git --all
```

## 파일상태를 unstage로 변경하기

```
$ git reset HEAD 파일명
```

modified단계에 있는 파일을 아예 처음으로 되돌리는 명령 수정한 내용은 복구 불가
```
$ git checkout -- 파일명
```
- 유저네임이 바뀌어서 주소가 바뀌었을 때.

1. 유저네임을 바꿔서 주소가 바뀌면 git remote rm origin 한 뒤 // remote "origin" 이름 지우기
2. git remote add origin 주소로  다시 설정 // remote "origin"이름 설정

```
$ git push origin [주소] // github 주소에 올리기 (리모콘으로 생각)
$ git remote -v // remote 추가 되었는지 확인 할 수 있다.
$ git push origin(어디에) master(무엇을) 
$ git clone [https 주소명] // 원격 저장소에 저장해둔 데이터 다시 다운받기. 즉 github폴더를 로컬로 불러오기
$ git clone [https 주소명] [폴더명] // 폴더명으로 https주소의 파일들을 불러 저장할 수 있다.
```

- 리모트 저장소에서 데이터를 가져오려면

```
git fetch [remote-name]
git merge origin/master
```

- fetch와 merge를 한번해 하기 위해서는

```
git pull
```
## git 태그

다음과 같이 tag v1.0을 지정해줄 수 있다.

```
git tag -a v1.0
```

git 태그 보기

```
git tag
git show v1.0
```
다음 두 명령은 차이점이 있다.

```
git push origin master
git push origin v1.0
```
> tag는 release를 할 때에 사용.

###branch
- 현재 브랜치가 무엇인지 확인

```
git branch
```

- testing이라는 브랜치를 만들 때.

```
git branch testing
```
- 브랜치 이름 바꾸기. (master브랜치에서 production브랜치로 이름 변경)

```
git branch -m master production
```
- 브랜치의 상태 확인.

```
git branch -v
```

- 현재 작업 중인 브랜치를 가리키는 HEAD.

> 'git log' 명령에 '--decorate' 옵션을 사용하면 쉽게 브랜치가 어떤 커밋을 가리키는지도 확인할 수 있다.

```
git log --decorate
```

- 브랜치로 이동하기

```
git checkout [다른 브랜치]
```
> 이렇게 하면 HEAD는 다른 브랜치를 가리킨다.

- git log 명령으로 갈라지는 브랜치를 쉽게 확인할 수 있다. 현재 브랜치가 가리키고 있는 히스토리가 무엇이고 어떻게 갈라져 나왔는지 보여준다.

```
git log --oneline --decorate --graph --all
```

> 위처럼 입력하면 히스토리가 그래프 형태로 출력되서 한눈에 알아보기 쉽다.

### git branch와 merge의 기초
브랜치와 merge의 기초
실제 개발과정에서 겪을 만한 예제를 하나 살펴보자. 브랜치와 merge는 보통 이런 식으로 진행한다.

1. 작업 중인 웹 사이트가 있다.
2. 새로운 이슈를 처리할 새 branch를 하나 생성한다.
3. 새로 만든 branch에서 작업을 진행한다.

이 때 중요한 문제가 생겨서 그것을 해결하는 Hotfix를 먼저 만들어야 한다. 그러면 아래와 같이 할 수 있다.

1. 새로운 이슈를 처리하기 이전의 운영(Production) 브랜치로 이동한다.
2. Hotfix 브랜치를 새로 하나 생성한다.
3. 수정한 Hotfix 테스트를 마치고 운영 브랜치로 Merge 한다.
4. 다시 작업하던 브랜치로 옮겨가서 하던 일을 진행한다.

- hotfix라는 브랜치를 만들면서 브랜치를 hotfix로 이동

```
git checkout -b hotfix
```

- 브랜치 확인

```
git branch -v
```

**파일 생성 및 add, commit 후
git checkout production으로 hotfix 이전으로 이동한 후 merge를 한다.**

```
git merge hotfix
```
**그 다음 필요 없게 된 hotfix를 지운다.**

```
git branch -d hotfix
```

## 충돌
> merge가 실패할 경우
\

<!--take-->