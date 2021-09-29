# Git

> 분산버전관리시스템

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





