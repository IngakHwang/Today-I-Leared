# Kotlin 문법정리

> 기초적인 문법정리



## 변수 선언

> 변수 = var
>
> 읽기전용변수 = val
>
> 상수 = const val 

코틀린의 타입추론(컴파일러가 변수에 할당된 데이터의 타입을 추론)으로 인하여 타입을 생략해도 될 때가 있다.



### var 변수

variable

`var 변수명: 타입`

ex) `var count: Int = 10`  -> Int타입의 변수 count에 10 선언



### val 읽기변수

value

`val 변수명: 타입`

ex) `val languageName: String = "Kotlin"` -> String 타입의 읽기전용변수 languageName 에 Kotlin 선언



### const val 상수

 `const val 상수명: 타입`

ex) `const val PI = 3.14` -> (타입추론) 상수PI에 3.14 선언



## 조건문

> 범위가 넓고 범위 자체를 한정 할 수 없다 -> if문
>
> 범위가 제한되고 값도 특정할 수 있다 -> when문



### if문

> java와 매우 유사
>
> 차이점 : 변수에 조건문 입력 가능, 한 줄 작성 가능

```kotlin
var a = 5
var b = 3

var bigger = if(a>b) a else b
```



### when문

> java의 switch문과 비슷하지만 좀 더 확장된 버전
>
> switch문은 `==` 로 비교
>
> when문은 `==` + 범위값도 처리 가능 등 더 다양하게 활용

```kotlin
// swich문과 같이 파라미터와 비교값 비교하여 조건문 수행
when(파라미터){
	비교값1 -> {
		// 파라미터와 비교값1 비교하여 같다면 이 code block 실행
	}
	비교값2 -> {
		// 파라미터와 비교값2 비교하여 같다면 이 code block 실행
	}
	else -> {
		// 앞서 비교한 코드들이 다르다면 else code block 실행
	}
}


// , 를 이용해 비교값을 한 번에 여러 개 비교
when(파라미터){
	비교값1,비교값2 ->{
		// 파라미터 = 비교값1 or 파리미터 = 비교값2 라면 이 code block 실행
	}
	else {
		// 앞에 비교한 코드들이 실행되지 않으면 else code block 실행
	}
}


// ... 을 이용해 조건의 범위를 지정 가능
when(파라미터){
	in 비교값1..비교값2 -> {
		// 파라미터가 비교값1, 비교값2 사이에 있다면 code block 실행
	}
	!in 비교값1..비교값2 ->{
		// 파라미터가 비교값1, 비교값2 사이에 없다면 code block 실행
	}
	else -> {
		// 위의 항목에 해당 하지 않다면 else code block 실행
	}
}


// if문 처럼 사용가능
var currentTime = 6
when {
	currentTime == 5 -> {
		println("현재 시간 5시")
	}
	currentTime >5 -> {
		println("5시 넘었음")
	}
	else ->{
		println("5시 이전")
	}
}
```



## 배열, 컬렉션

> 하나의 변수에 여러 개의 값을 담을 수 있게 해주는 데이터 타입



### 배열

array

> 하나의 변수에 여러개의 정해진 값을 담을 수 있고
>
> 처음 정해진 값의 개수는 바꿀 수 없는 데이터 타입
>
> `Array`, `arrayof`, `타입Array`, `타입Arrayof`

```kotlin
var array = Array(10)
var intArray = IntArray(10)
var longArray = LongArray(10)
var charArray = CharArray(10)
var floatArray = FloatArray(10)
var doubleArray = DoubleArray(10)

//String은 기본 타입은 아지만 조금 특이 하게 사용 가능
var stringArray = Array(10, {item->""})

//값을 선언과 동시에 입력 arrayOf
var dayArray = arrayOf("월","화","수","목","금","토","일")
var intArrayOF = intArrayof(1,2,3,4,5,6,7,8,9,10)

//배열에 값 입력         배열명[인덱스] = 값, set 함수 
dayArray[6] = "일요일"
dayArray.set(5,"토요일")

//배열 값 꺼내기         배열명[인덱스] , get 함수
var seventhValue = intArrayOf[6]
var tenthValue = intArrayOf.get(9)
```



### 컬렉션

collection

> 동적으로 크기가 변경할 수 있도록 만들어진 동적 배열

코틀린에서 동적으로 리스트를 사용하기 위해서는 컬렉션 앞에 Mutable접두어가 붙는다.

접두어가 없는 컬렉션도 있지만 잘 사용하지 않는다.

주로 `mutableList` , `mutableMap`, `mutableSet` 사용

---

**mutable - 변할 수 있는 데이터 타입을 가르키는 용어**

코틀린은 데이터 타입을 설계 할 때 모두 immutable로 설계되었다.

list, map, set 모두 한 번 입력된 값을 바꿀 수 없어, 컬렉션의 원래 용도인 동적 배열을 사용하기 위해 mutable로 만들어진 데이터 타입을 사용한다.

----



#### list

> 저장되는 데이터에 인덱스를 부여한 컬렉션
>
> 중복된 값 입력 가능
>
> `mutableListOf<type>`

```kotlin
// list 생성
var list = mutableListof("월", "화", "수")

// list 값 추가
list.add("목")

// list 값 사용
var variable = list.get(1).       // variable = 화

// list 값 수정
list.set(0,"월요일")

// list 값 삭제
list.removeAt(0)

// 값이 아무 것도 없는 list 선언
var 변수명 = mutableListOf<타입>()
var newList = mutableListOf<String>()

// list 크기
var listSize = list.size          // listSize = 3
```



#### set

> 중복을 허용하지 않는 리스트
>
> 인덱스 조회 불가
>
> get 함수 없음
>
> `mutableSetOf<type>`

```kotlin
// set 생성
var set = mutableSetOf<String>()

// set 값 추가
set.add("월")
set.add("수")
set.add("금")
set.add("금") // (X) 동일한 값 입력할 수 없음

// set 값 사용 (인덱스 조회 불가로 특정 위치의 값 직접 사용 X)
println("Set 전체 출력 = ${set}")        //Set 전체 출력 = [월,수,금]

// set 값 삭제 (값이 중복되지 않아 직접 조회 후 삭제)
set.remove("금")
```



#### map

> Key, Value 쌍으로 입력되는 컬렉션
>
> `mutableMapOf<type, type>`

```kotlin
// map 생성
var map = mutableMapOf<String, String>()

// map 값 추가
map.put("key1", "value1")
map.put("key2", "value2")
map.put("key3", "value3")

// map 값 사용 (get 함수 사용)
var mapValue = map.get("key2")          // mapValue = value2

// map 값 수정 (기존 키 수정)
map.put("key3", "value3 수정")

// map 값 삭제
map.remove("key3")
```



---

**엘리먼트(Element) : 컬렉션 값의 단위**

컬렉션에 입력되는 값은 엘리먼트라고 한다.

값이라도 해도 되지만 맵의 값을 가르키는 건지 엘리먼트의 실제 값을 가르키는 건지, 용어 충돌로 인하여 구분한다.

list element = list value

map element = map key,value

-----



## 반복문



### for

> 특정 횟수만큼 코드 반복

1. 특정 횟수 만큼 반복
2. 시작값, 종료값 범위 만큼 반복 (for in)
3. 시작값~ 종료값에서 종료값 이전까지만 반복 (for in until)
4. step - 건너뛰기
5. downTo - 감소시키기
6. 배열, 컬렉션의 엘리먼트 반복

```kotlin
//1
for (반복할 범위){
	// 실행코드
}

//2
for (변수 in 시작값..종료값)[
	//실행 코드
}

//2 ex)
for (index in 1..10){
	println("현재 숫자는 ${index}")          //1~10 출력
}

//3 ex)
for (index in 1 until 10){
	println("현재 숫자는 ${index}")          //1~9 출력
}

//4 ex)
for (index in 0..100 step 3){
	println("현재 숫자는 ${index}")          //0,3,6,9 ... 99
}

//5 ex)
for (index in 10 downTo 0){
	println("현재 숫자는 ${index}")          //10~0 출력
}

//6 ex
var arrayDay = arrayOf("월", "화", "수", "목", "금")
for (day in arrayDay){
	println("현재 ${day}요일 입니다. "
}
```



### while

> 특정 조건 만족 할 때 까지 반복

1. 특정조건 만족 할 때 까지 반복
2. dowhile → do 실행 후 반복문 실행

```kotlin
//1
while(조건식){
	// 실행코드
}

//2
do{
	// 실행코드
}while(조건식)
```



### repeat

> N번 반복

```kotlin
var number : int = 3
repeat(number){
		println("${number}번 반복")
}
```



#### break

> 반복문을 완전히 벗어나기



#### continue

> 반복문 도중에 다음 반복문으로 넘어가기

```kotlin
for (except in 1..10){
	if(except > 3 && except < 8){
		continue
	}
	println("${except}")
}
//1,2,3,8,9,10 출력
```



## 함수

> 하나의 특별한 목적의 작업을 수행하기 위해 독립적으로 설계된 코드의 집합
>
> 모든 코드는 함수 안에 작성해야 하며 코드의 실행은 함수를 호출하는 것



### 함수 선언

- 함수 파라미터 정의 시 여러 개의 파라미터 정의 할 경우는 콤마 `,` 로 구분한다.
- 코틀린 함수 파라미터는 모두 val이 생략된 형태
- 파라미터 정의 시 등호 `=` 사용하여 설정가능
- 파라미터 이름으로 값 입력가능

```kotlin
// 기본구조 (파라미터, 반환타입 없다면 생략가능)
fun 함수명(파라미터 이름: 타입): 반환타입 {

}

//1 반환값O, 입력값O
fun square(x: Int): Int{
	return x * x
}

//2 반환값X, 입력값O
fun printSum(x: Int, y: Int){
	println("x+y = ${x+y}")
}

//3 반환값O, 입력값X
fun getPi(): Double{
	return 3.14
}

//4 반환값X, 입력값X
fun justFun(){
	println("Hello")
}

//여러 개의 파라미터
fun sample((val 생략)name: String, age: Int, height: Double){}

//등호 사용하여 파라미터 설정
fun sample2(name: String, age: Int = 30, height: Double){
	println("name : ${name}")
	println("age : ${age}")
	println("height : ${height}")
}
sample2("David",180.0)

//파라미터 이름으로 값 입력
sample2("Michael", height = 170.0)
```



### 함수 사용

> 함수 사용 시 반드시 괄호 `()` 를 붙여 실행

```kotlin
//1 반환값O, 입력값O
var squareResult = sqaure(30)          //squareResult 변수 안에 900

//2 반환값X, 입력값O
printSum(3,5)                          //x+y = 8 출력

//3 반환값O, 입력값X
val PI = getPi()                       //PI 상수 안에 3.14

//4 반환값X, 입력값X
justFun()                              //Hello 출력
```



## 클래스

> 그룹화 할 수 있는 변수와 함수의 모음, 인스턴스의 설계도

**기본 선언 구조**

```kotlin
//클래스 기본구조
class 클래스명{
	var 변수
	fun 함수(){
		// code
	}
}
```



### 생성자

> 클래스를 사용하기 위해서는 생성자라고 불리는 함수가 호출

코드를 실행하는 것 = 함수를 호출하는 것

클래스도 마찬가지, 클래스를 사용한다는 것은 곧 클래스 라는 이름으로 묶여 있는 코드를 실행하는 것이기에 함수 형태로 제공되는 생성자를 호출해야지만 클래스가 실행된다.

 

#### 프라이머리 생성자

> 클래스 이름 옆에 괄호로 둘써아인 코드로 주 생성자 라고 부름

프라이머리 생성자는 마치 클래스의 헤더처럼 사용할 수 있으며 constructor 키워드를 사용해서 정의하는데, 조건에 따라 생략이 가능하다.



클래스의 생성자가 호출되면 init 블록의 코드가 실행되고,

init 블록에서는 생성자를 통해 넘오온 파라미터에 접근 할 수 있다.



대신 파라미터로 전달된 값을 사용하기 위해선 `val` 키워드를 붙여주면 클래스 스코프 전체에서 해당 파라미터를 사용 할 수 있다. 

```kotlin
//프라이머리 생성자 기본 구조
class Person constructor(value: String){

}

//생성자에 접근 제한자나 다른 옵션이 없다면 constructor 키워드 생략
//파라미터로 전달된 값을 사용 시 val 키워드를 붙여 클래스 전역에서 사용하자
class Person(val value: String){
	init{
		println("생성자로부터 받은 값 : ${value}")
	}
}
```



#### 세컨더리 생성자

> 클래스 블록 내에 존재하는 생성자
>
> constructor 키워드를 함수 처럼 작성
>
> init 블록을 작성하지 않고 constrctor 다음에 괄호를 붙여 사용

```kotlin
class Person{
	constructor(value: String){
		println("생성자로부터 받은 값 : ${value}")
	}
}
```



파라미터의 개수, 파라미터의 타입이 다르다면 여러 개를 중복해서 만들 수 있음

```kotlin
class Person{
	constructor(value: String){
		println("생성자로부터 받은 값 : ${value}")
	}
	constructor(value: Int){
		println("생성자로부터 받은 값 : ${value}")
	}
	constructor(value1: Stirng, value2: Int){
		println("생성자로부터 받은 값 : ${value1}, ${value2}")
	}
}
```



#### defalut

> 생성자를 작성하지 않았을 때
>
> 파라미터가 없는 프라이머리 생성자가 하나 있는 것과 동일

```kotlin
class Person{
	init{
	
	}
}
```



---

참고사이트

Kotlin 생성자 정리 velog : https://velog.io/@conatuseus/Kotlin-%EC%83%9D%EC%84%B1%EC%9E%90-%EB%BF%8C%EC%8B%9C%EA%B8%B0

----



### 클래스 사용 | 인스턴스

> 클래스 사용 => 클래스명 ()
>
> 클래스의 생성자를 호출한 후 생성되는 것 => 인스턴스
>
> 생성된 인스턴스는 변수에 담아 둘 수 있다. 

```kotlin
var one = Person("value")
var two = Person(1004)
```



#### 프로퍼티, 메서드

> 프로퍼티 : 클래스 내부에 정의된 멤버 변수
>
> 메서드 : 클래스 내부에 정의된 멤버 함수
>
> 프러퍼티, 메서드 사용 -> 인스턴스가 담긴 변수명 다음에 .붙여 사용

`pig.name` ,`pig.printName()`



### object

> java의 static = kotlin의 object
>
> 클래스를 인스턴스화 하지 않아도 블록 안의 프로퍼티와 메서드를 호출해서 사용

```kotlin
object Pig{
    var name: String = "Pinky"
    fun printName(){
        println("Pig의 이름 : ${name}")
    }
}

Pig.name = "Micky"
Pig.printName()						// Pig의 이름 : Micky
```



#### companion object

> 일반 클래스에 object 기능 추가하기 위해 사용
>
> companion object 블록으로 감싸주면 생성과정 없이 object 처럼 사용 가능

```kotlin
class Pig {
	companion ojbect {
		var nmae: String = "None"
		fun printName(){
			println("Pig의 이름은 ${name}")
		}
	}
	fun walk(){
		println("Pig가 걸어 간다.")
	}
}

// companion object 안의 코드 사용
Pig.name = "Linda"
Pig.printName()                   //Pig의 이름은 Linda

// companion object 밖의 코드 사용
val cutePig = Pig()
cutePig.walk()                    //Pig가 걸어간다. 출력
```



### 데이터 클래스

> 간단한 값의 저장 용도로 사용
>
> 주로 네트워크를 통해 데이터를 주고 받거나
>
> 로컬 앱의 DB에서 데이터를 다루기 위한 용도로 사용

일반 클래스와 사용법이 거의 유사

데이터 클래스를 정의할 때 class 앞에 data 키워드 사용

생성자 파라미터 앞에 var, val 생략 불가능



일반 클래스와 차이점

toString() 메서드 : 데이터 클래스의 값을 반환

copy() 메서드로 값 복사 가능

```kotlin
data class 클래스명 (val 파라미터1 : 타입, var 파라미터2: 타입)

//정의 - 주로 코드 블록을 사용하지 않고 간단하게 작성
data class UserData(val name: String, var age: Int)

//생성 - 일반 클래스의 생성자 함수를 호출하는 것과 동일
var userData = UserData("David", 21)

//val로 선언된 name은 변경 불가능
userData.name = "Sindy" (X)

//var 로 선언된 age는 변경 가능
userData.age = 18

println("DataUser는 ${userData.toString()}")
// DataUser는 (name=David, age=18)

var newData = userData.copy()
```



### 상속

> 부모 클래스의 프로퍼티, 메서드를 재사용 가능

상속 대상이 되는 부모 클래스를 open 키워드로 만들면 자식 클래스에서 사용 할 수 있다.

open키워드가 있지 않으면 상속 불가능

상속 받을 자식 클래스는 콜론 : 을 사용해 상속할 부모 클래스 지정



상속은 부모의 인스턴스를 자식이 갖는 과정이기에 부모 클래스명 다음에 괄호를 입력해서 꼭 부모의 생성자를 호출해야 한다.

```kotlin
open class 부모 클래스{
	
}

class 자식 클래스: 부모클래스(){

}
```



부모 클래스의 생성자에 파라미터가 있다면

자식 클래스의 생성자를 통해 값을 전달 할 수 있다.

```kotlin
open class 부모 클래스(value: String){

}

//자식 클래스의 value가 부모 클래스 파라미터를 전달
class 자식 클래스(value: String): 부모 클래스(value){

}
```



부모 클래스에 세컨더리 생성자가 있다면

자식 클래스의 세컨더리 생성자에서 super키워드로 부모 클래스에 전달 할 수 있다.

자식 클래스에 세컨더리 생성자만 있을 경우 상속되는 클래스 이름 다음에 괄호가 생략

```kotlin
// ex) android view class
class CustomView: View{
	constructor(ctx: Context): super(ctx)
	constructor(ctx: Context, attrs: AttributeSet): super(ctx, attrs)	
}
```



부모 클래스의 프로퍼티나 메서드 중 자식 클래스에서 다른 용도로 사용해야 하는 경우 Override 하여 사용

오버라이드 할 떄 프로퍼티나 메서드도 클래스 처럼 open이 붙여 있어야 한다.

```kotlin
open class BaseClass{
	open var openedVar: String = "I am"	

	open fun opened(){}
	fun notOpened(){}
}

class childCalss: BaseClass(){
	override var openedVar: String = "You are"
	override fun opened(){}
	override fun notOpened(){} // (X) open키워드가 없어 불가
}
```



익스텐션

미리 만들어져 있는 클래스에 메서드를 넣는 개념

클래스, 메서드, 프로퍼티에 대한 익스텐션

이미 만들어져 있는 클래스에 메서드를 추가 할 수 있다.

```kotlin
fun 클래스.확장할 메서드(){}

class MyClass{
	fun say()
	fun walk()
	fun eat()
}

MyClass.sleep(){ }

fun.sleep()
```



### 패키지

> 연관성 있는 클래스와 소스 파일을 관리하기 위한 디렉토리 구조와 저장 공간



### 추상화

> 클래스를 개념 설계하기 위한 도구

사전적의미 : 주어진 문제나 시스템을 중요하고 관계 있는 부분만 분리해 내어 간결하고 이해하기 쉽게 만드는 작업

설계 단계에서 코드를 작성하지 않고 메서드의 이름만 작성



### 인터페이스

> 외부 모듈에 제공하기 위해 메서드 이름을 나열한 명세서
>
> 실행 코드 없이 메서드 이름만 가진 추상 클래스



### 접근 제한자

> 서로 다른 파일에게 자신에 대한 접근 권한을 제공하는 것

기본적으로 아무런 예약어가 없다면 public

#### private 

> 다른 파일에서 접근 X



#### internal

> 같은 모듈에 있는 파일만 접근

모듈 -> 한 번에 같이 컴파일 되는 모든 파일, 라이브러리도 모듈

#### protected

> private하지만, 상속관계에서 자식 클래스가 접근



#### public

> 모든 파일 접근 가능 (default)



```kotlin
open class Parent(){
	private val privateVal = 1
	protected open val protectedVal = 2
	internal val internalVal = 3
	val defaultVal = 4
}

class Child: Parent(){
	//privateVal 호출 불가능
	println("protected 변수 = ${protectedVal}") //상속 관계이기에 호출
	println("internal 변수 = ${internalVal}")
	println("defalutVal 변수 = ${defalutVal}")
}
```



### 제너릭

> 타입을 특정해서 안전성을 유지하기 위한 설계 도구

일반적인 코드를 작성하고, 이 코드를 다양한 타입의 객체에 대하여 재사용하는 프로그래밍 기법

클래스에서 사용할 타입을 클래스 외부에서 설정하는 타입



#### 제너릭 약자

T → Type

E → Element

N → Number

K → Key

V → Value

