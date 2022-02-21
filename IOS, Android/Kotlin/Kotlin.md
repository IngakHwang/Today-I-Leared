# Kotlin

> 코틀린은 자바 플랫폼에서 돌아가는 새로운 프로그래밍 언어이다.
>
> 기존 자바 라이브러리나 프레임워크와 함께 잘 작동하며, 성능도 자바와 같은 수준이다.



## Kotlin 특성

코틀린의 주 목적 : 자바가 사용되고 있는 모든 용도에 적합하면서 더 간결하고 생상적이고 안전한 대체 언어를 제공하는 것



코틀린을 활용할 수 있는 가장 일반적인 영역 : 서버상의 코드 (웹 어플리케이션 백엔드), 안드로이드 모바일 앱

코틀린 영역은 상당히 광범위 하다. => 이유는 잘 모르겠음



## 변수, 상수 선언

> 변수 = var, 상수 = val



변수

 `var count : Int = 10`  => Int 타입의 변수 count에 10 초기화



상수

`val languageName : String = "Kotlin"` String 타입의 상수 languageName에 Kotlin 초기화



## 조건문

> if, elseif 문은 자바와 거의 동일하다.
>
> `when` 표현식 : 조건문을 조건, 화살표 ->, 결과로 표시

```kotlin
// if, elseif
if(count == 42) {
	println("1")
} else if(count > 35){
	println("2")
}else {
	println("X")
}

// when 표현 (왼쪽 조건이 true 일 시, 화살표 오른쪽 표현식 결과 반환
// 결과 반환 후 다음 분기로 실행되지 않는다?? 
val answerString = when {
	count == 42 -> "1"
	count > 35 -> "2"
	else -> "X"
}
```



## 함수

> `fun 함수이름(파라미터이름 : 타입) : 반환타입 {함수내용}`  - 파라미터 없으면 생략 가능 

```kotlin
//파라미터가 없고 return타입 String인 generateAnswerString 함수
fun generateAnswerString() : String {
	val answerString = if (count == 42) {
		"1"
	} else {
		"X"
	}

	return answerString
}

val answerString = generateAnswerString()


//파라미터(파라미터이름 : 타입) 있고 return타입 String인 generateAnswerString 함수
fun generateAnswerString(countThreshold : Int) : String {
	val answerString = if (count > countThreshold){
		"1"
	} else{
		"X"
	}

	return answerString
}

val answerString = generateAnswerString(42)

//변수 리턴이 아닌 바로 리턴시키기
fun generateAnswerString(countThreshold : Int) : String {
	return if (count > countThreshold) {
		"1"
	} else {
		"X"
	}
}
```









----

참고사이트

안드로이드 공식사이트 : https://developer.android.com/kotlin/learn