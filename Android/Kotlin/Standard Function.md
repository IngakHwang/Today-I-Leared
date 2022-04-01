# Standard function

> Kotlin에서 제공하는 기본적인 함수들

Scope function만 있는 줄 알았지만 takeif, takeunless도 있고

심지어 repeat 또한 standard function 이다.



## Scope function

> 특정 객체에 대한 작업을 블록 안에 넣어 실행 할 수 있도록 하는 함수
>
> 범위를 일시적으로 만들어 속성, 함수를 처리하는 용도로 사용되는 함수

특정 객체에 대한 작업을 블록안에 넣게 되면 가독성이 증가하고 유지보수가 쉬워진다.



스코프 함수 : 특정 객체의 컨텍스트 내에서 특정 동작을 실행하기 위한 목적만을 가진 함수

스코프 함수를 람다 함수로 사용하게 되면 임시로 스코프를 형성한다.

이 스코프 내에서 객체의 이름을 통해 일일이 참조할 필요 없이 객체를 접근하고 핸들링 할 수 있다.



Kotlin에서 주로 쓰는 Scope function이 있다.

`apply` , `run` , `with` , `let`, `also` 



처음 코틀린을 공부하는 입장에서 비슷비슷한 5개의 함수를 구별하기 위해 각 function에 대한 기준을 세웠다.

1. 'it'을 쓰는지, 'this'를 쓰는지
2. 리턴값이 function의 마지막줄의 리턴인지, 객체 인지



## Summary

`apply` : this, 객체 리턴

`run` : this, 함수 실행

`with` : this, 함수 실행, 파라미터 사용

`let` : it, 함수 실행, null 체크

`also` : it, 객체 리턴



### apply

> 객체 내부 프로퍼티를 변경한 다음 객체를 반환하는 함수

객체를 생성 및 초기화 할 때 사용된다.

```kotlin
fun main(){
	var a = Book("공부법", 10000).apply{
		name = "[폭탄세일중] "+ name
		discount()
	}

	println("상품명: ${a.name}, 가격: ${a.price}")
	//결과
	//상품명: [폭탄세일중] 공부법, 가격: 8000
}

class Book (var name: String, var price: Int) {
	fun discount(){
		price = price - 2000
	}
}
```



### run

> 객체를 return하지 않고, run 블록의 마지막 라인을 return

객체에 대해 특정한 동작을 수행한 후 결과값을 리턴 받아야 할 경우 사용한다.



```kotlin
fun main(){
	var a = Book("공부법", 10000).apply{
		name = "[폭탄세일중] "+ name
		discount()
	}

	var bookCost = a.run {
		println("상품명: ${a.name}, 가격: ${a.price}")	
		price + 2000
		// price + 2000 이 반환 (10000)
	}
	println("원가는 $bookCost 입니다.")
	
	//결과
	//상품명: [폭탄세일중] 공부법, 가격: 8000
	//원가는 10000 입니다
}

class Book (var name: String, var price: Int) {
	fun discount(){
		price = price - 2000
	}
}
```



run은 객체 없이도 동작 할 수 있다.

```kotlin
val person = Person(name = "David", age = 29, temperature = 36.5f)
val isPersonSick = run{
    person.temperature = 37.2f
    checkPersonIsSick(person)
}
```



### with

> run과 동일하게 동작한다. 차이점은 with는 객체를 파라미터로 받는다.

```kotlin
// run
var bookCost = a.run{
	println("상품명: ${a.name}, 가격: ${a.price}")
	price + 2000
}

//with
var bookCost = with(a){
	println("상품명: ${a.name}, 가격: ${a.price}")
	price + 2000
}
```



### let

> 객체를 이용해 마지막 줄을 return 한다. it을 사용한다.

null check 후 코드를 실행해야 할때, -> ?.let

nullable한 수신객체를 다른 타입의 변수로 변환해야 할 때 사용한다.

객체를 명시할 수 있고, 명시하지 않으면 it으로 사용한다.

```kotlin
//let 사용X
val alice = Person("Alice",20,"Amsterdam")
println(alice)        //Alice, 20, Amsterdam
alice.moveTo("London")
alice.incrementAge()
println(alice)        //Alice, 21, London

//let 사용
Person("Alice",20,"Amsterdam").let{
	println(it)         //Alice, 20, Amsterdam
	it.moveTo("London")
	it.incrementAge()
	println(it)         //Alice, 21, London
}

//let 사용 객체 명시
Person("Alice",20,"Amsterdam").let{ alice ->
	println(alice)         //Alice, 20, Amsterdam
	alice.moveTo("London")
	alice.incrementAge()
	println(alice)         //Alice, 21, London
}
```



### also

> let과 유사, 명령 수행 후 인스턴스 반환, it 사용한다.

```kotlin
//let과 also 비교
fun main(){
	data class Person(var name: String, var skills: String)
	
	var person = Person("Kildong", "Kotlin")
	
	//let
	val a =person.let{
		it.skills = "Android"
		"success"
	}
	
	println(person)        //Person(name=Kildong, skills=Android)
	println("a: ${a}")     //a: success

	//also
	val b = person.also{
		it.skills = "Java"
		"success"
	}

	println(person)        //Person(name=Kildong, skills=Java)
	println("b: ${b}")     //b: Person(name=kildong, skills=Java)
}
```



## 그 외



### takeIf

> null이 아니거나 조건이면 작업 수행
>
> 조건함수가 true이면 this 반환
>
> false이면 null 반환

```java
public inline fun <T> T.takeIf(predicate: (T) -> Boolean): T? 
    = if (predicate(this)) this else null
```

1. T 의 확장함수 이다. 즉, `T.takeIf` 로 사용 할 수 있다.
2. takeIf의 조건 함수 predicate는 파라미터로 T 객체를 전달 받는다.
3. takeIf의 조건 함수 predicate 의 결과에 따라 T 객체인 자기 자신(this) 나 null 을 반환한다.



ex)

```kotlin
//원본 코드
if (someObject != null && someObject.status){
  someObject.doThis()
}

//개선 코드
someObject?.takeIf{ status }?.doThis()
```





### takeUnless

> 조건함수가 true이면 null 반환
>
> false이면 this 반환

```java
public inline fun <T> T.takeUnless(predicate: (T) -> Boolean): T? 
    = if (!predicate(this)) this else null
```

takeUnless 함수는 takeIf 함수와 역으로 동작한다.

takeUnless 는 조건함수가 true 일 때 null을 반환, false 일 때 자기 자신(this)를 반환한다.





---

참고사이트

[코틀린] 많이 쓰이는 표준함수 let, also, apply, run, with 의 차이 블로그 : https://velog.io/@ejjjang0414/%EC%BD%94%ED%8B%80%EB%A6%B0-%EB%8C%80%ED%91%9C%EC%A0%81%EC%9D%B8-%ED%91%9C%EC%A4%80%ED%95%A8%EC%88%98-let-also-apply-run-with-%EC%9D%98-%EC%B0%A8%EC%9D%B4



Kotlin World : https://kotlinworld.com/255



Kotlin: 스코프 함수들(Scope functions) let, run, with, apply, also 블로그 : http://batmask.net/index.php/2021/12/10/286/



언제 뭘 써야돼? 헷갈리는 스코프 함수들 velog : https://velog.io/@haero_kim/%EC%96%B8%EC%A0%9C-%EB%AD%98-%EC%8D%A8%EC%95%BC%EB%8F%BC-%ED%97%B7%EA%B0%88%EB%A6%AC%EB%8A%94-%EC%8A%A4%EC%BD%94%ED%94%84-%ED%95%A8%EC%88%98%EB%93%A4-%ED%95%9C-%EB%B0%A9-%EC%A0%95%EB%A6%AC#4-also--let



takeif, takeUnless medium : https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%9D%98-takeif-takeunless-%EB%8A%94-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80-f6637987780