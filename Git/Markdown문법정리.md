# Markdown문법정리

## **Heading - 제목**

​	 **#으로 제목**	

​	문서의 제목이나 소제목으로 사용

​	문서의 구조를 위해 작성, 글자 크기를 조절하기 위해 사용X

​	#의 개수에 따라 제목크기가 달라짐 (h1~h6)

​	**How?**

​	  `# Heading1`,` ## Heading 2`



## **List - 리스트** 

​	순서가 있는 리스트(ol), 순서가 없는 리스트(ul)로 구성	

​	**How?**	

​	순서가 있는 리스트 ex) `1. first item`, `2. second item`

​	순서가 없는 리스트 ex) `- first item` `- second item`

​	

​	**Tip**

​	Typora 에서는 tab으로 하위 항목, Shift+tab으로 상위항목 작성



## **Fenced Code Block - 코드 필기 할 때**

​	backtick기호(키보드 1옆, tab위) 3개를 활용하여 작성

​	코드 블록에 특정 언어를 명시하면 Syntax Highlighting 적용 가능

​	Syntax Highlighting - 언어 내 변수, 예약어, 리터럴 색깔 등 색깔을 다르게 표시해주는 기능

​	ex) ```(언어입력)

​	````java`

```java
int a = 1;
a = a+1;
```



## **Inline Code Block - 키워드 표시 할때**

​	backtick 기호 1개를 인라인에 활용하여 작성

​	ex)

​	java에서 `if`문을 사용할때 => 키워드를 표시할때 

​	`class`는 무엇이다

```java
`inline block`
```



## **Link - 링크내용과 링크 표시 할때, 파일 주소 적을 떄**

​	ex)

​	My favorite search engine is [네이버](www.naver.com)

`[문자열](url)`

​	

## **Image - 이미지 끌어다쓰기**

​	`![문자열](url)`로 사용 가능한대

​	이미지 끌어다 써도 댐

![둘리_초능력맛좀](../md-images/%EB%91%98%EB%A6%AC_%EC%B4%88%EB%8A%A5%EB%A0%A5%EB%A7%9B%EC%A2%80.JPG)

## **Blockquotes(인용문) - 한줄정리 할때 사용**

​	`>`으로 한줄평 하기 좋음

​	ex)

> 기계어는 기계가 알아먹는 언어



## **Table - 데이터 정리 시**

​	Typora 에서는 ctrl + t로 생성가능

| 제목 | 소제목 | 소제목 |
| ---- | ------ | ------ |
| 4    | 1      | 2      |
| 5    | 1      | 2      |
| 6    | 1      | 2      |



## **text 강조 - 굵게, 기울기**

​	굵게는 ctrl + b or `**글자**`

​	기울기는 crtl + i or `*글자*`



## **수평선**

​	3개 이상의 asterisks (***), dashes (---), or underscores (___)

******************************

-----------------------

________

