# Git

> 분산버전관리시스템, 코드의 버전을 관리하는 도구

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







