# HTML 웹 프로그래밍

## 인터넷(Internet)

> 전 세계를 연결하는 국제 정보 통신망

컴퓨터나 스마트폰 같은 디지털 기기로 연결되어 사람들이 정보를 공유할 수 있는 공간



## 웹 (Web)

> 인터넷 공간에서 제공하는 서비스

웹은 **요청**과 **응답** 이라는 간단한 형태로 동작

요청자 - 클라이언트

응답자 - 서버



웹은

클라이언트가 서버에 파일을 요청하면

서버가 해당 요청에 응답해 요청한 파일을 클라이언트에게 제공하는 통로



## 웹 브라우저 (Web Browser)

> 웹에서 공개된 정보를 탐색할 수 있게 하는 프로그램



## 하이퍼링크 (Hyper Link)

> 인터넷에서 문서 사이를 쉽게 이동 할 수 있는 기능



## URL (Uniform Resource Locator)



## URI (Uniform Resource Identifier)



## 3 Tier Programing 

> Client - Server - DB
>
> 프로트엔드 - 미들웨어 - 백엔드



## 서버 프로그램 (Server Program)

> 클라이언트 요청에 따라 적절한 파일, 데이터를 제공



## 클라이언트 프로그램 (Client Program)

> 웹 브라우저에서 작동하는 프로그램

서버에서 전달된 HTML파일을 읽는 소프트웨어



HTML=> 요소 생성

CSS => 디자인

JS => 프로그래밍요소 부여



## HTML (Hyper Text Markup Language)

> 웹 페이지를 구성하는 HTML 마크업 언어

마크업(Markup) - 웹 페이지의 서식이나 구조를 표현하는 정보

### 기본용어

요소 (element) ⇒ HTML 페이지를 구성하는 각 부품

ex) 제목, 본문, 이미지 등 모두 요소

태그(tag) ⇒ 이러한 요소를 만들 때 사용하는 작성 방법

ex) **<h1>** 제목 **</h1>**

속성(attribute) ⇒태그에 추가 정보를 부여할 떄 사용하는 것

ex) <img **src = "../data/image.png"**>



### HTML TAG

#### HTML 기본구조

```html
<!DOCTYPE html>
<html>
<head>
	<title>Title</title>
</head>
<body>

</body>
</html>
```

`<!DOCTYPE html>` ⇒ 웹 브라우저에게 HTML5문서라고 알림

`<html>` ⇒ HTML페이지 기본요소, 모든 태그는 html태그 안에 작성

`<head>` ⇒ body태그에 필요한 스타일시트, 자바스크립트 제공

`<title>` ⇒ 웹 브라우저에 표시되는 제목 지정

`<body>` ⇒ 사용자에게 실제로 보이는 부분 작성하는 곳



#### `<head>` 내부에 넣을 수 있는 태그

`<meta>` => 웹페이지에 추가 정보 전달

`<title>` => 웹페이지 제목 지정

`<script>` => 웹페이지에 JS 연결

`<link>` => 웹페이지에 다른 파일 연결

`<style>` => 웹페이지에 CSS 연결

`<base>` => 웹페이지의 기본 경로 지정



#### 글자태그

`<h1>~<h6>` => 제목 글자 생성 (닫는 태그)

`<p>` => 본문 문단 생성 (닫는 태그)

`<br>` => 줄 바꿈

`<hr>` => 수평 줄 삽입

`<a>` => 하이퍼링크(닫는 태그)

a의미 = anchor

href 의미 = hyper reference

```html
<a href ="naver.com"> 네이버링크 </a>

<a href = "../folder/1.html"> 사이트 내 다른 페이지 가기</a>

<a href ="#"> 빈 링크 </a>
```



`<b>` => 굻은 글자(닫는 태그)

`<i>` => 기울어진 글자(닫는 태그)



#### 목록태그

`<ul>` => 순서가 없는 목록 (닫는 태그) unordered list

`<ol>` => 순서가 있는 목록 (닫는 태그) ordered list

`<li>` => 목록 요소 (닫는 태그)list item

```html

<body>
<--! 순서목록이 없는 li -->
	<ul>
		<li>사과</li>
		<li>바나나</li>
		<li>오렌지</li>
	</ul>

<--! 순서목록이 있는 ol -->
	<ol>
		<li>사과</li>
		<li>바나나</li>
		<li>오렌지</li>
	</ol>

<--! li+ol -->

<ul>
과일
	<ol>
		<li>사과</li>
		<li>바나나</li>
		<li>오렌지</li>
	</ol>
채소
	<ol>
		<li>상추</li>
		<li>치커리</li>
		<li>양배추</li>
	</ol>
</ul>
</body>


```



#### 테이블 태그

`<table>` => 표삽입

`<tr>` => 표에 행 삽입 table row

`<th>` => 표의 제목셀 table heading

`<td>` => 표의 내용셀 table data

th,td 속성

colspan => 가로로 결합 (너비지정)

rowspan => 세로로 결합 (높이지정)



`<thead>` => 표의 제목

`<tbody>` => 표의 내용

```html
<body>
	<table>
		<thead>
			<tr>
				<td></td>
				<td>월</td>
				<td>화</td>
				<td>수</td>
				<td>목</td>
				<td>금</td>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>1교시</td>
				<td>영어</td>
				<td>국어</td>
				<td>과학</td>
				<td>미술</td>
				<td>미술</td>
			</tr>
			<tr>
				<td>2교시</td>
				<td>도덕</td>
				<td>체육</td>
				<td>영어</td>
				<td>수학</td>
				<td>사회</td>
			</tr>
		</tbody>
	</table>
</body>
```



```html
<body>
	<table border="1">
	<tr>
		<td>엔티티타입명</td>
		<td colspan="4">부서</td>
		<td>작성일</td>
		<td colspan="2">2011년 10월 01일</td>
	</tr>
	
	<tr>
		<td>테이블명</td>
		<td colspan="4">DEPT</td>
		<td>작성자</td>
		<td colspan="2">홍길동</td>
	</tr>
	
	<tr>
		<td>테이블 설명</td>
		<td colspan="7">부서의 위치와 역할 정보를 관리한다.</td>
	
	</tr>

	<tr>
		<td>번호</td>
		<td>컬럼명</td>
		<td>속성명</td>
		<td>데이터타입</td>
		<td>길이</td>
		<td>NULL 여부</td>
		<td>기본값</td>
		<td>KEY</td>
	</tr>
	
	<tr>
		<td>1</td>
		<td>DEPTNO</td>
		<td>부서번호</td>
		<td>NUMBER</td>
		<td>2</td>
		<td>NOT NULL</td>
		<td></td>
		<td>PK</td>
	</tr>
	
	<tr>
		<td>2</td>
		<td>DEPTNM</td>
		<td>부서명</td>
		<td>VARCHAR2</td>
		<td>14</td>
		<td>NOT NULL</td>
		<td></td>
		<td></td>
	</tr>
	
	<tr>
		<td>3</td>
		<td>LOC</td>
		<td>위치</td>
		<td>VARCHAR2</td>
		<td>14</td>
		<td>NULL</td>
		<td></td>
		<td></td>
	</tr>
	
	<tr>
		<td>4</td>
		<td>CHARGE</td>
		<td>역할</td>
		<td>VARCHAR2</td>
		<td>20</td>
		<td>NULL</td>
		<td></td>
		<td></td>
	</tr>

	</table>
</body>
```



#### 미디어 태그

`<img>` => 이미지 삽입

속성

src ⇒ 이미지 경로 지정

alt ⇒ 이미지 없을 시 글자 지정

width ⇒ 이미지 가로

height ⇒ 이미지 세로



```html

<body>
	<img src ="image.jpg" alt="이미지" width="100" height="100">
	<img src ="../folder/image.jpg"> <!--상대경로-->
</body>

<!--../ 상위 디렉토리 -->
<!--./ 현재 디렉토리 -->
```



#### 입력양식 태그

> 사용자에게 정보를 입력받는 요소

`<form>` => 입력 양식 시작과 끝 태그 (닫는 태그)

​	속성

​		action => data를 보내는 url 입력

​		method => data 보내는 방식 (GET or POST)



`<input>` =>사용자에게 입력받는 요소

​	속성

​		type

​			text ⇒ 글자 입력 양식

​			button ⇒ 버튼

​			checkbox ⇒ 체크박스 (다중 선택)

​			password ⇒ 비밀번호

​			radio ⇒ 라디오 (단일 선택)

​			file ⇒ 파일

​			reset ⇒ 초기화버튼

​			submit ⇒ 제출버튼



​	name => Key 지정



`<textarea>` => 여러 글자 입력 받는 요소 (닫는 태그)

​	속성

​		rows ⇒ 세로 크기

​		cols ⇒  가로 크기



`<select>` => 아이템 선택양식 (닫는태그) - `option` 태그와 세트

`<option>` => 아이템 - `select` 태그와 세트

`<label>` => 속성 for에 지정된 id와 연결



```html
<body>

<!--  3(column) X (row) 10 테이블 생성 -->
<form action="" method="">
	<table>
		<tr>
			<td colspan=3>데이터수집</td>
		</tr>
		
		<tr>
			<td>이름</td>
			<td><input type='text' name="user_name" value="kim"></td>
			<td></td>
		</tr>
		
		<tr>
			<td>아이디</td>
			<td><input type='text' name="user_id" value="ghkdrnjsm"></td>
			<td><input type="button" value="중복검사"></td>
		</tr>
		
		<tr>
			<td rowspan="2">패스워드</td>
			<td><input type='password' name="password" value="1234"></td>
			<td></td>
		</tr>
		
		<tr>
			<td><input type='password' name="password2" value="1234"></td>
			<td>
				<button>패스워드 확인</button>
			</td>
		</tr>
		
		<tr>
			<td>전화번호</td>
			<td>
			<!-- <input type='text' size="3" name="phone_no1"> - -->
			<select name="phone_no1">
				<option>--선택--</option>
				<option selected="selected">010</option>
				<option>051</option>
				<option>02</option>
			</select>
			 
			<input type='text' size="3" name="phone_no2"> - 
			<input type='text' size="3" name="phone_no3">
			</td>
			<td></td>
		</tr>
		
		<tr>
			<td>이메일</td>
			<td>
				<input type='text' size="10" name="email1">@
				<input type='text' size="10" name="email1">
			</td>
			<td>
				<select name="email">
					<option>--이메일 선택--</option>
					<option>nate.com</option>
					<option selected="selected">naver.com</option>
					<option>google.com</option>
				</select>
			</td>
		</tr>
		
		<tr>
			<td>사용언어</td>
			<td>
				<input type='checkbox' name="langs" checked="checked">자바
				<input type='checkbox' name="langs">파이썬
				<input type='checkbox' name="langs">자바스크립트
				<input type='checkbox' name="langs">C#
			</td>
			<td><button>다중선택확인</button></td>
		</tr>
		
		<tr>
			<td>사용툴</td>
			<td>
				<input type='radio' name="tools">이클립스
				<input type='radio' name="tools" checked="checked">메모장
				<input type='radio' name="tools">VSC
			</td>
			<td><button>단일선택확인</button></td>
		</tr>
		
		<tr>
			<td>비고</td>
			<td>
				<textarea rows="5" cols="30" name="etc">Welcome</textarea>
			</td>
			<td></td>
		</tr>
		
		<tr>
			<td colspan=3>
				<input type="submit" value="전송">
				<input type="reset" value="다시작성">
			</td>
	
		</tr>
		
	</table>
</form>

</body>
```



#### 공간분할 태그

`<div>` => Block (한줄)

`<span>` => Inline (쓰는 만큼)



#### 시맨틱 태그

> 의미가 있는 태그

시맨틱(semantic) : 의미론적인

header, nav, section, footer 등등 웹 페이지 구역 나누는 태그



`<header>` => 머리말 (페이지 제목, 소개)

`<nav>`=> 네비게이션

`<aside>` => 본 게시물 사이드 항목

`<section>` => 본 게시물

`<article>` =>본문과 독립적인 컨텐츠

`<footer>` => 꼬리말 (저자나 저작권 정보)
