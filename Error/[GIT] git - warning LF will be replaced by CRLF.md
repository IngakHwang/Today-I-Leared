# [GIT] git - warning: LF will be replaced by CRLF

> 형상 관리를 해주는 git이 바라볼 땐 어느 쪽을 선택할지 몰라 경고 메세지를 띄워준 것

| 영문                                                         | 해석                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| warning : LF will be replaced by CRLF in {파일명}. The file will have its original line endings in your working directory | 경고 : {파일명}에서 LF는 CRLF로 대체됩니다. 파일은 작업 디렉토리에 원래 줄 끝이 있습니다. |



## LF (Line-Feed)

- Mac, Linux(Unix 계열) 줄바꿈 문자열 = \n
- ASCII 코드 = 10
- 커서 위치는 그대로 두고 종이의 한라인 위로 올리는 동작
- 현재 위치에서 바로 아래로 이동
- 종이를 한칸 올리기



## CR (Carriage-Return)

- Mac 초기 모델 줄바꿈 문자열 = \r
- ASCII 코드 = 13
- 커서 위치를 맨 앞으로 옮기는 동작
- 커서 위치를 앞으로 이동



## CRLF (Carriage-Return + Line-Feed)

- Windows, DOS 줄바꿈 물자열 = \r\n
- CR(\r) + LR (\n) 두 동작을 합친 것
- 커서를 다음라인 맨 앞으로 옮겨주는 동작



문서의 끝을 처리하는 데 있어서 OS마다 약간의 차이가 있기 때문에 발생한다.



유닉스 시스템에서는 한 줄의 끝이 LF (Line Feed)로,

윈도우에서는 줄 하나가 CR(Carriage Return)와 LF (Line Feed), 즉 CRLF로 이루어지기 때문이다.



유닉스 OS (Mac) 을 쓰고 있다면 CRLF will be replaced by LF in ... 에러 메시지가 뜰 것이고,

윈도우를 쓰고 있다면 LF wii be replaced by CRLF in ... 에러 메시지 뜰것이다.



## 해결방안

CRLF 와 LF를 자동 변환해주는  `core.safecrlf` 기능

```bash
$git config --global core.autocrlf true
```



리눅스, 맥을 사용하고 있는 경우, 조회 할 떄 LF를 CRLF로 변환하는 것을 원하지 않을 것이다.

따라서 뒤에 `input` 명령어를 추가해줌으로써 단방향으로만 변환이 이루어지도록 설정한다.

```bash
$git config --global core.autocrlf true input
```



혹은 변환 기능을 원하지 않고, 에러 메시지 끄고 알아서 작업하고 싶은 경우 `safecrlf` off

```bash
$git config --global core.safecrlf false
```





---

참고사이트

블로그1 : https://jaddong.tistory.com/entry/git-add-%EC%97%90%EB%9F%AC-warning-LF-will-be-replaced-by-CRLF-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95

블로그2 : https://blog.jaeyoon.io/2018/01/git-crlf.html

블로그3 : https://dabo-dev.tistory.com/13