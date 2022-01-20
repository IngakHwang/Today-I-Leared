# Git

> 분산버전관리시스템, 코드의 버전을 관리하는 도구



## 버전 관리 시스템

> 파일 변화를 시간에 따라 기록하고 특정 시점 버전을 다시 꺼낼 수 있는 시스템

버전 관리 시스템 (VCS - Version Control System)

왜 필요할까? ex)

1. 100만장에 종이 중 무작위 5장에 A라고 적었다.
2. 그 5장에 B라고 적고 섞는다.
3. B라 적은 내용을 새로 고쳐 C라고 써야한다.

버전 관리 시스템이 없을 때에는 일일이 찾아서 수정해야 함

버전 관리 시스템이 있을 때에는 각 상황마다 버전을 기록하고 관리해두었다면 그냥 1번 버전으로 돌아간 후 수행



좀 더 쉽게 말하면 **뒤로가기가 가능** 하다는 의미



### VCS 종류



#### Local VCS

> 파일을 각 시점마다 복사해서 저장하는 것

![image-20220120165816322](md-images/image-20220120165816322.png)



위의 방식은 아주 간단하고 쉽지만, 변형되기도 쉽고 삭제, 변경에 대해 취약해서 잘못되어도 알아내기 어렵다



이를 더 쉽게 할 수 있게 하고자 간단한 DB에 파일의 변경 정보를 담아 관리하는 로컬 VSC라는 것이 만들어졌다.

![image-20220120165954785](md-images/image-20220120165954785.png)

위와 같은 방식인데 모든 것이 로컬 컴퓨터에 있기에 다른 컴퓨터에서 해당 DB에 접근하기 어렵다.

이러한 문제를 해결하기 위해 개발된 것이 CVCS (Centralized Version Control Systems) 이다.



#### CVCS

> 파일 관리를 하는 중앙 서버에 DB가 있어 로컬에서 해당 서버에서 파일을 받아서 사용

CVCS (Centralized Version Control Systems)

![image-20220120170154015](md-images/image-20220120170154015.png)

파일 관리를 하는 중앙 서버에 DB가 있어 로컬에서 해당 서버에서 파일을 받아서 사용한다.

각 로컬에서 관리하지 않고 중앙에서 관리하기에 관리가 쉽고 서버 기록을 보면 누가 무엇을 했는지 알 수 있다.



다만 단점은 중상 서버가 작동하지 않으면 각 버전의 정보를 받아올 수 없고, 중앙 서버 하드에 문제가 생기면 각 로컬로 가져간 스냅샷 (어느 특정한 버전, 상태에 대한 기록)은 무사하지만 전체적인 히스토리를 잃을 수 있다.

commit 한 내용이 바로 중상서버로 업로드 되는 방식이므로 그 소스를 공유하는 모든 작업자에게 영향이 간다.



이러한 문제를 개선된 것이 DVCS (Distributed Version Control Systems) 이다.



#### DVCS

> 로컬 작업공간에서 작업을 진행하다가 중간중간 로컬저장소로 올려서 작업한 내용에 대한 이력을 관리하고 개발이 끝나서 해당 파일을 최종적으로 반영하고 할 때, 그 때 중앙서버에 업로드하는 방식

![image-20220120170954284](md-images/image-20220120170954284.png)

CVCS는 commit -> 중앙서버 업로드 였다면, DVCS (git)은 최종적으로 중앙서버에 올리기 까지 몇 가지의 과정을 더 거쳐야 한다.



그 과정에서 나온느 개념이 로컬저장소이고, 로컬 작업공간에서 작업을 진행하다가 중간중간 올리고자 하는 내용을 로컬저장소로 올려서 작업한 내용에 대한 이력을 관리 할 수 있다.

개발이 끝나서 해당 파일을 최종적으로 반영하고자 할 때, 그 때 중상서버에 업로드하는 방식이다.

git의 기능은 많고, 로컬 저장소에 올리기 전에도 commit할 대상을 지정할 수 있는 스테이지 영역도 있는데, 진행해야 하는 작업이 많은 경우 작업별로 commit 대상을 분리 할 수 있는 기능이다.



`로컬 작업 공간 -> 스테이지 영역 -> 로컬저장소 -> 중앙서버` 형태로 구성 



**명령 Summary**

|                            | SVN    | git                                        |
| -------------------------- | ------ | ------------------------------------------ |
| 중앙서버의 내용 내려받기   | update | pull (fetch + merge를 한 번에 처리할 경우) |
| 스테이지 영역에 추가       |        | add                                        |
| 로컬저장소로 내용 올리기   |        | commit                                     |
| 로컬저장소로 내용 내려받기 |        | fetch                                      |
| 로컬저장소에서 내용 합치기 |        | merge                                      |
| 중앙서버로 내용 올리기     | commit | push                                       |



## git 저장소 만들기

```bash
$ git init
Initialized empty Git repository in C:/Users/student/Desktop/test/.git/

(master) $
```

* `.git` 폴더가 생성되며, 버전이 관리되는 저장소
* git bash에서는 `(master)` 로 브랜치가 표기 된다. 

## 버전 만들기

### `add`

> 변경된 사항 저장하기

```bash
$ git add <파일/폴더/디렉토리>
$ git add . # 현재 디렉토리 변경 모두
$ git add a.txt   # 특정 파일
$ git add folder/ # 특정 폴더 
$ git add *.png   # 특정 확장자
```

#### 예시

```bash
$ touch a.txt
$ git status
On branch master

No commits yet
# 트래킹되지 않는 파일 / Working directory	(아직 add되지 않음)
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.txt

nothing added to commit but untracked files present (use "git add" to track)
$ git add a.txt
$ git status
On branch master

No commits yet
# 커밋될 변경사항들!!! (Staging area)		(add된 파일)
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   a.txt

```

### `commit` 

> add된 파일이 어떻게 변경되었는지 기록

```bash
$ git commit -m '변경사항기재'
```

#### 예시

```bash
$ git commit -m 'first'
[master (root-commit) acca762] first
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 a.txt
```

## 상태 관련 명령어

### `status`

> add되었는지 확인 할때

```bash
$ git status
```

#### 예시

```bash
$ git status
On branch master

No commits yet
# 커밋될 변경사항 => git add가 된 파일 (commit 해야 함)
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   1.txt
# 아직 트래킹되지 않은 파일 => git add가 안된 파일 (add 후 commit)
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.txt
```



### `log`

> commit된 기록조회

```bash
$ git log
```

#### 예시

```bash
$ git log
commit 4bb326437f7f90946011db0a1c42c0f2af403531 (HEAD -> master)
Author: IngakHwang <Ingak.Hwang@users.noreply.github.com>
Date:   Wed Sep 29 15:17:30 2021 +0900

    third

commit 62768e158d452e71da854e13959fca92ec12f962
Author: IngakHwang <Ingak.Hwang@users.noreply.github.com>
Date:   Wed Sep 29 15:16:30 2021 +0900

    second

commit acca762f4d0a4da00b7af2516825f08c2465247e
Author: IngakHwang <Ingak.Hwang@users.noreply.github.com>
Date:   Wed Sep 29 15:13:57 2021 +0900

    first
```

`git log -1` : 최근 기록된 커밋 조회

```bash
$ git log -1
commit 4bb326437f7f90946011db0a1c42c0f2af403531 (HEAD -> master)
Author: IngakHwang <Ingak.Hwang@users.noreply.github.com>
Date:   Wed Sep 29 15:17:30 2021 +0900

    third
```

## 원격 저장소 관련 명령어

### `git remote add origin "저장소 url"`

> 원격저장소(Github) 등록

```bash
$ git remote add origin https://github.com/IngakHwang/first.git
```

origin은 변수명 이라고 생각⇒ 바꿀 수 있는데, 왠만하면 바꾸지 않는 것이 좋음

### `git remote -v`

> 원격저장소 주소 확인

```bash
$ git remote -v
origin  https://github.com/IngakHwang/first.git (fetch)
origin  https://github.com/IngakHwang/first.git (push)
```

### `git push origin master`

> commit된 내용 Github으로 push 

```bash
$git push -u origin master		#master = branch
```

### `git pull origin master`  

> Github 저장소 땡겨오기

```bash
$git pull -u origin master
```

### `git clone 'url'`

> github에 주소에 있는 Repositories 가져오기

`clone` 은 원격저장소 자체를 가져온다



## 브랜치 관련 명령어

### `git branch {branch name}` 

> 브랜치 생성

```bash
$ git checkout 'newBranch'
```

### `git checkout {branch name}` 

> 현재 브랜치에서 {branch name}으로 이동

```bash
~/Desktop/branch (feature/index)  #(feature/index => master 이동)
$ git checkout master
Switched to branch 'master'
```

### git checkout -b {branch name}

> {branch name} 생성 및 이동

```bash
~/Desktop/branch (master)			#(feature/index 브랜치생성 후 이동)
$ git checkout -b 'feature/index'
Switched to a new branch 'feature/index'
```

### `git branch` 

> branch 목록 조회

```bash
$ git branch
* master
```

### `git branch -d {branch name}`

> {branch name} 삭제

```bash
$ git branch -d feature/about
Deleted branch feature/about (was a61ecd3).
```

### `git merge {branch name}`

>  브랜치 병합 = merge 

```bash
$ git merge feature/index
Updating 139169d..15e4ff5
Fast-forward
 index.html | 0
 1 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 index.html
```

### `git long --ineline --graph` 

> 브랜치 가지 확인

```bash
$ git log --oneline --graph
*   20e3e0e (HEAD -> master) Merge branch 'feature/ppt'
|\
| * 95d5731 (feature/ppt) Add ppt & Update README
* | 80a8bfd  인각
|/
*   8db1722 README change
|\
| * a61ecd3 Update README ...
* | 5c6d968 Update README
|/
*   5bcea52 Merge branch 'feature/style'
|\
| * 9d11cef complete index css
* | 3891a94 Hotfix
|/
* 25601f7 Use VSC for commit
* c574e0f branch README commit
```

## Undoing

### `git restore --staged {파일이름}` 

> add 취소

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md
        modified:   a.txt
```

```bash
$ git restore --staged a.txt
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt
```



### `git restore {파일이름}` 

> 작업 내용을 이전 버전 상태로 되돌리기
>
> ***해당 명령어 실행 후 다시 복원 불가능***

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  # 변경사항을 버리기 위해서는...
  # WD 있는거..
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
$ git restore a.txt
```



### `git commit --amend` 

> 커밋 메시지 수정
>
> 공개된 저장소에 push 된 커밋 메시지 수정하면 큰 일남
>
> => 커밋 해시값이 변경됨 (기존 버전을 건든다)



## README.md

오픈소스의 공식 문서를 작성하거나

개인 프로젝트의 프로젝트 소개서로 활용



----

참고 사이트 : 

git 간편 안내서 : https://rogerdudler.github.io/git-guide/index.ko.html

git 관련 블로그 : https://ojava.tistory.com/157

git book : http://git-scm.com/book/ko/v2

git 관련 블로그 : http://marklodato.github.io/visual-git-guide/index-ko.html
