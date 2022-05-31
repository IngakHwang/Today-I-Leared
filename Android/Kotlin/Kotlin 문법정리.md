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



#### get(), set()

`var` 는 변수, `val` 는 상수가 아니다. 둘 다 변수 이다.



코틀린 클래스에서 `val` ,`var` 로 정의되는 변수를 프로퍼티라고 한다.

자바에서의 멤버 변수인 field 와는 다르다.



프로퍼티는 `field + getter/setter 메소드` 라고 볼 수 있다.

프로퍼티를 생성하면 getter / setter가 자동으로 생성되기 때문이다.

(private val/var은  자바의 field 와 동일하다.)

(private은 getter, setter를 만들지 않기 때문이다.)



코틀린에서 프로퍼티를 정의하면 getter/setter 함수를 만들지 않고 그냥 직접 접근하여 사용하면된다.

하지만 실제로 직접 접근하지 않는다.

내부적으로 `get()`, `set()` 을 사용하며 코틀린 코드에서는 이 부분이 생략이 되었다고 볼 수 있다.



코틀린의 프로퍼티는 다음과 같은 규칙으로 getter / setter 를 생성한다.

- val 은 불변 (immutable) 이기 때문에 getter 만 생성된다.
- var 은 변하기 (mutable) 때문에 getter / setter 가 모두 생성된다.
- getter의 이름은 `get + 변수이름` , setter의 이름은 `set + 변수이름` 으로 정해진다.
- `isStudent` 처럼 변수 이름에 is가 붙는다면, getter는 `is + (is를 제거한 이름)` , setter는 `set + (is를 제거한 이름)` 이 된다.
- private 변수는 getter / setter가 생성되지 않는다.



ex)

```kotlin
// val get() 사용

class test {
  val name : Int
  	get(){
      return this.age
    }
}
```



##### 커스텀 get, set

프로퍼티를 정의하면 getter / setter 가 생성된다.

변수에 값을 변경하거나 리턴만 한다.

다른 계산, 로그 출력 등의 코드가 없지만 추가 할 수 있다.

프로퍼티에 `get()`, `set()` 함수를 정의하면 된다.



```kotlin
class Rectangle {
  var width = 10
  	set(value){
      field = value / 2						//field = 해당 변수, value = 입력 값
    }
  
  var height = 10
  	set(value){
      field = value / 2
    }
  var area : Int = 0
  	get() = width * height
}

fun main(){
  val rect = Rectangle()
  println("width: ${rect.width}")								//width : 10
  println("height: ${rect.height}")							//height : 10
  println("area: ${rect.area}")									//area : 100
  
  rect.width = 40
  rect.height = 40
  println("width: ${rect.width}")								//width : 20
  println("height: ${rect.height}")							//height : 20
  println("area: ${rect.area}")									//area : 400
  
}
```



##### private setter

지금까지 만든 프로퍼티는 외부에서 변수를 변경하고(set), 읽을 수 있었다(get).

getter, setter를 모두 만들었는데, 외부에서 변수를 변경(set)하지 못하도록 할 수 있다.



`set()` 을 private 으로 선언하면 된다.

```kotlin
class Rectangle {
    var width = 10
        private set(value) {
            field = value / 2
        }
    var height = 10
        private set(value) {
            field = value / 2
        }
    var area: Int = 0
        get() = width * height
}

// 외부에서 객체의 변수를 변경할 수 없습니다
val rect = Rectangle()
//rect.width = 40
//rect.height = 40
```



----

참고사이트 : https://codechacha.com/ko/kotlin-property/

---



## Print , Log 값 접근

> Print나 Log를 할 때 값을 변수에 담지 않고 `$` 키워드나 `${}` 를 사용하여 값 접근

ex)

**Java**

```java
String text = "테스트변수"
println("text 변수는 "+text)

//text 변수는 테스트변수
  
Log.d("LOG", "text - "+text)
```



**Kotlin**

```kotlin
val text : String = "테스트변수"
println("text 변수는 $text")

//text 변수는 테스트변수

Log.d("LOG", "text - $text")
```





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



### data class

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



**익스텐션**

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



#### 상속 변경자

Java에서는 classs는 기본적으로 상속이 가능

상속을 불가능하게 만들기 위해서는 `final` 변경자를 붙어야 한다.

객체 지향 관점에서는 객체가 있고 해당 객체에 대한 코드를 줄이기 위해 재사용 가능하다면 재사용 하는 것이 좋지만, 이러한 사용 방식은 상속하는 기반 클래스가 변경이 없는 경우에만 유효

기반 클래스 (부모)가 변경이 잦은데 무분별하게 클래스를 상속하게 된다면, 취약한 기바 클래스(fragile base class) 문제에 직면하게 된다.

기반 클래스가 변경될 때마다 상속한 모든 자식 클래스들은 변경되어야 하며, 어느 문제가 생길지 모른다.



이런 문제를 해결하기 위해 코틀린에서는 class 의 기본 상속 변경자를 `final` 로 설정

만약 특정 클래스가 상속되어도 되는 경우에만 `open` 이라는 변경자를 이용하면 상속 가능



**`final`** : 상속이 불가능한 변경자

**`open`** : 상속이 가능한 변경자

**`abstract`** : 상속을 해야만 인스턴스화 가능한 변경자



```kotlin
open class Tab()
class GalaxyTab() : Tab()			// final 생략

abstract class Pad(){
  var szie = 10
  fun getSizeString() : String = "{$size}cm"
}

class Ipad : Pad()
val ipad = Ipad()

println(Ipad.getSizeString())		// 10cm
```



----

Kotlin 상속 변경자 : https://kotlinworld.com/68?category=925008

----

### 패키지

> 연관성 있는 클래스와 소스 파일을 관리하기 위한 디렉토리 구조와 저장 공간



### 추상화

> 클래스를 개념 설계하기 위한 도구

사전적의미 : 주어진 문제나 시스템을 중요하고 관계 있는 부분만 분리해 내어 간결하고 이해하기 쉽게 만드는 작업

설계 단계에서 코드를 작성하지 않고 메서드의 이름만 작성

----

**생활코딩 - 자바수업**

abstract : 추상

상속을 강제하는 일종의 규제라고 먼저 생각하자.

abstract 클래스나 메소드를 사용하기 위해선 반드시 상속해서 사용하도록 강제하는 것이 abstract 이다.



추상 메소드 (abstract method)

메소드의 시그니처만이 정의된 비어있는 메소드이다.

구체적인 구현은 하위 클래스에서 오버라이딩 해야 한다.



추상 클래스 (abstract class)

상속을 강제하기 위한 것이다.

메소드의 시그니처만 정의해놓고, 메소드의 실제 동작 방법은 메소드를 상속받은 하위 클래스의 책임으로 위임하는 것이다.



ex) templeate method pattern

![img](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/1972.png)

이 그림에 등장하는 템플릿은 자주 사용하는 모양을 모아둔 것이라고 할 수 있다.

템플릿은 모양을 결정하지만 템플릿을 통해 그려질 도형은 팬의 종류나 색상에 따라 달라진다.

즉, 추상화 메소드가 어떻게 동작할지 알 순 없지만 실행은 할 것이다.

하지만 실행결과는 하위 클래스가 결정한다.



----



####  Sealed class

Super class를 상속받는 Child 클래스의 종류 제한하는 특성을 갖고 있는 클래스이다.

어떤 클래스를 상속받는 하위 클래스는 여러 파일에 존재할 수 있기 때문에 컴파일러는 얼마나 많은 하위 클래스들이 있는지 알지 못한다.

하지만 Sealed class 는 동일 파일에 정의된 하위 클래스 외에 다른 하위 클래스는 존재하지 않는다는 것을 컴파일러에게 알려주는 것과 같다.



ex)

`Color` 라는 상위 클래스를 만들고, 동일한 파일에 이 클래스를 상속하는 `Red`, `Blue` 라는 클래스를 선언했다고 가정

Sealed class는 이 두개의 클래스 외에 `Color` 클래스를 상속받는 다른 클래스는 없다라는 것을 컴파일러에게 말해준다.



이렇게 하위 클래스가 될 수 있는 클래스를 제한하여 얻을 수 있는 장점 중 하나는 `when` 을 사용할 때 `else` 를 사용하지 않는 것이다.

이것은 코틀린에서 제공하는 Enum으로도 얻을 수 있는 이점이다.

하지만 Enum은 `Red` 라는 하위 객체를 Singleton 처럼 1개만 생성할 수 있고 복수의 객체는 생성할 수는 없다.

반면에 Sealed class는 1개 이상의 객체를 생성할 수 있다.



**정의 방법**

```kotlin
sealed class Color

object Red : Color()
object Green : Color()
object Blue : Color()


//사용
val color : Color = Red
val color2 : Color = Color.Red
```



**특징**

- 클래스 앞에 `sealed` 키워드를 붙여 정의
- Sealed class 는 abstract 클래스로, 객체로 생성할 수 없다.
- Sealed class의 생성자는 private이고, public으로 설정할 수 없다.
- Sealed class와 그 하위 클래스는 동일한 파일에 정의되어야 한다. 서로 다른 파일에서 정의할 수 없다.
- 하위 클래스는 `class` , `data class` , `object class` 으로 정의 할 수 있다.



**이점**

하위 클래스를 제한해서 얻는 이점 중 하나는 `when` 을 사용할 때 이다.

`when` 은 모든 케이스에 대해서 처리가 되어야 하기 때문에 `else` 구문이 꼭 들어가는데, 

`Sealed class` 를 사용하면 컴파일 시점에 하위 클래스들이 정해져 있기 때문에, 모든 하위 클래스에 대한 케이스를 구현하면 `else` 구문을 추가하지 않을 수 있다.



----

참고사이트 코드 차차 - https://codechacha.com/ko/kotlin-sealed-classes/

---



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



### enum class

> 관련 있는 상수들의 집합 class
>
> 상태 모드 등 유사한 값들을 고유한 값으로 만들어 사용하기 위해 사용한다.
>
> `enum class 클래스명(){}`

Enum => Eumeration : 열거형 => 서로 연관된 상수들의 집합



- 코드가 간략해지며 가독성이 높아진다.
- 인스턴스 생성과 상속을 방지하여 타입 안전성 보장한다.
- 구현 의도가 명확해진다.
- 상태 모드 등 유사한 값들을 고유값으로 만들어 사용하기 위해서 사용한다.



구현

- `enum class 클래스명(){}`
- 클래스 안에 메소드 정의 시 상수 목록과 메소드 정의 사이에 `;` 넣어야 한다.

ex)

```kotlin
enum class Color (val r: Int, val g: Int, val b: Int){
	RED(255, 0, 0),
	GREEN(0, 255, 0),
	BLUE(0, 0, 255);

	fun rgb() = (r * 256 + g) * 256 + b
}

fun colorName(color : Color) =
	when(color){
		color.RED = "빨강"
		color.GREEN = "초록"
		color.BLUE = "파랑"
	}
	
fun main(){
	println(colorName(Color.BLUE))       //"파랑"
}
```

-----

참고 사이트

1. https://json8.tistory.com/170
2. https://tourspace.tistory.com/99?category=797357

-----




## 블록

> 프로그램 코드에서 블록 (block) 이란 마치 한 문단처럼 보이는, 코드의 한 부분을 뜻함
>
> 중괄호로 묶여 있는 경우가 많다.

블록 = 코드 블록 = Unit

```kotlin
// 전형적인 블록
fun answerString() : String 
// 블록 시작
{
	var hello = "hello"
	return hello
}
// 블록 끝
```



## nullable, Non-nullable

> 코틀린은 자바와 다르게 Nullable, Non-nullable 타입으로 프로퍼티를 선언 할 수 있다.
>
> Non-nullable 타입으로 선언 시 객체가 null이 아닌 것을 보장하기 때문에 null check 코드 작성을 할 필요가 없다.



> 코틀린은 null에 안전하도록 설계 되었다.



코틀린 기본 변수는 모두 null 입력이 되지 않는다.



### `?`  => null 허용

> null 입력받기 허용
>
> 타입 뒤에 `?` 작성 => nullable (null 할당 가능)
>
> 없으면 non-Nullable (null 할당 불가능)
>
> `var 변수명 : 타입?` 

```kotlin
var nullable: String? = "nullable"        //nullable
var nonNullable: String = "non-Nullable"  //non-Nullable
```

함수 파라미터에도 null 허용 여부를 설정할 수 있다.

null을 허용하려면 해당 파라미터에 대한 null 체크를 먼저 해야만 사용가능

```kotlin
fun nullParameter(str: String?){
	if(str != null){
		var length2 = str.length
	}
}
```

함수 리턴 타입에도 nullable 여부를 설정 가능

```kotlin
fun nullReturn(): String?{
	return null
}
```



### `!!` => null 아니라는 것 보장

> null이 아니라는 것을 보장할 수 있는 객체에만 사용
>
> 메서드 뒤에 사용

```kotlin
// nonNullString 은 Non-nullable
// getString() 은 nullable  이라고 가정 한다.

//컴파일 에러
//String?타입을 String에 할당하려고 했기 때문
var nonNullString: String = getString()  

//컴파일 성공
var nonNullString: String = getString()!!
```



### `?.` => safe call (값이 null 이 아닐 때 값 호출)

> 값이 Null 인지 체크 후 실행을 위함
>
> null이 아니면 `?.` 다음에 나오는 속성, 명령어 처리
>
> null이면 null 리턴
>
> Safe call 연산자 (안전하게 nullable 프로퍼티 접근)

```kotlin
val a: String = "Kotlin"
val b: String? = null
println(b?.length)      //b가 null 이라면 length를 호출하지 않고 null 리턴
println(a?.length)      //if(a==null) return null else a.length
```



```kotlin
fun testSafeCall(str: String): Int?{
	//str이 null이면 length를 체크하지 않고 null을 반환
	var resultNull: Int? = str?.length
	return resultNull
}
```



### `?:` => elvis (값이 null 일 때, 아닐 때 리턴 값이 다르다.)

> null이 아니면 `?:` 왼쪽 값 리턴
>
> null이면 `?:` 오른쪽 값 리턴
>
> 엘비스 연산자 (Elvis Operation)
>
> 삼항 연산자와 비슷하다.

```kotlin
//null이 아니라면 ?: 왼쪽 객체 리턴
//null이라면 ?:의 오른쪽 객체 리턴
val 1 = b?.length ?: -1
```



```kotlin
fun testElvis(str: String): Int{
	//length 오른쪽에 ?: 사용하면 null 일 경우 ?: 오른쪽의 값이 반환
	var resultNonNull: Int = str?.length?:0
}
```



## 지연 초기화

> 초기화 타이밍을 늦추는 효과
>
> 코틀린은 코드 초반에 초기화 해야 하는데 그 초기화를 나중에 하겠다.



### `lateinit` => var 전용 지연 초기화

> 클래스 안에서 변수 (프로퍼티)만 미리 선언하고 초기화 (생성자 호출)를 나중에 해야 할 경우



- var로 선언된 클래스의 프로퍼티에만 사용 가능
- null은 허용되지 않는다.
- 기본 자료형 (Int, Long, Double 등등) 에서 사용 불가



`lateinit` 은 변수를 미리 선언만 해 놓은 방식이기에 초기화되지 않는 상태에서 메서드나 프로퍼티 참조 시 null 예외 발생

​	=> 이러한 문제로 잘 사용하진 않는다.



예시)

```kotlin
class Person{
	lateinit var name: String
	init {
		name = "Lionel"
	}
	fun process(){
		name.plus(" Messi")
		print("이름 길이 = ${name.length}")
		print("이름 첫글자 = ${name.substring(0,1}")
	}
}
```



### `lazy` => val 전용 지연 초기화

> lazy 초기화된 항목은 실제 실행 시 메모리에 올라감으로 메모리 입장에서 좋음
>
> 자주 쓰인다.



변수 선언 후 코드 뒤 쪽에 `by lazy` 키워드 붙여 사용

```kotlin
class Test{
  val myName : String by lazy {
    "John"
  }
}
```



----

`lateinit` ,`lazy` 참고 사이트 : https://medium.com/til-kotlin-ko/kotlin-delegated-property-by-lazy%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80-74912d3e9c56

----





## Scope function, Standard function

> 범위를 일시적으로 만들어 속성, 함수를 처리하는 용도로 사용되는 함수

스코프 함수 : 특정 객체의 컨텍스트 내에서 특정 동작 을 실행하기 위한 목적만을 가진 함수



스코프 함수를 람다 함수로 사용하게 되면 임시로 스코프를 형성한다.

이 스코프 내에서 객체의 이름을 통해 일일이 참조할 필요 없이 객체를 접근하고 핸들링 할 수 있다.

`?` 남용을 막아주는 역할도 한다.



1. ‘it’ 을 쓰는지, ‘this’ 를 쓰는지 ⇒ it은 인자명 지정 가능, 기본적으로  it 으로 접근
2. 리턴값이 함수 실행인지, 객체 인지



it을 쓰는 이유 => 변수명을 지정해서 보는 사람이 보기 편하도록 하기 위함



`apply` : this, 객체 리턴

`run` : this, 함수 실행

`with` : this, 함수 실행, 파라미터 사용

`let` : it, 함수 실행

`also` : it, 객체 리턴



-----

Scope function 사용에 대한 참고 사이트

1. https://medium.com/androiddevelopers/kotlin-standard-functions-cheat-sheet-27f032dd4326
2. https://medium.com/@kimtaesoo188/mastering-kotlin-standard-functions-run-with-let-also-and-apply-7fb4492db246

-----



## Method Chaining

> 메소드 리턴 객체를 받고 이 리턴 객체의 메소드를 호출하는 방법을 반복 (메소드를 체인으로 엮듯이)



> 둘 이상의 메서드 호출 또는 속성 엑세스를 단일 표현식으로 결합하는 것
>
> Method Chaining 사용 시 임시 변수의 사용을 줄일 수 있다.
>
> 코드를 더 쉽게 읽고 유지 관리 할 수 있다.

ex)

```kotlin
// No Chained method calls
Double cost = binding.costOfService
String costString = cost.toString()

// Chained method calls
String costString = binding.costOfService.toString()
```



## Pair, Triple

함수는 1개의 객체를 리턴한다.

하지만 간혹 2개의 값을 리턴하고 싶을 때가 있다.

이럴 때 클래스를 만들어 그 안에 필요한 값을 저장하고 리턴하거나, 배열을 만들어 그 안에 저장하고 리턴할 수 있다.



코틀린은 Pair, Triple 객체를 기본으로 제공하여 이런 불편함을 해소하려고 한다.

Pair는 2개의 객체를 저장할 수 있는 객체이고,

Triple은 3개의 객체를 저장하는 객체이다.



### 사용법

> Pair는 객체를 생성하고 2개의 객체를 넣을 수 있다.
>
> 안에 들어가는 객체의 클래스 또는 자료형은 중요하지 않고 두개가 동일해도 되고 달라도 된다.



```kotlin
val pair1 = Pair("Hello", "World")
val pair2 = Pair("Hello", 1234)
```

타입을 생략할 수 있지만 명시적으로 타입을 쓸 수도 있다.

```kotlin
val pair3 = Pair<String, String> ("Hello", "World")
val pair4 = Pair<String, Int> ("Hello", 1234)
```



Pair 안에 저장된 객체는 `first` , `second` 로 접근 할 수 있다.

```kotlin
val pair = Pair("Hello", 1234)
println(pair.first)					//Hello
prinlnt(pair.second)				//1234
```



새로운 변수를 정의할 때 Pair를 이용하여 쉽게 값을 할당 할 수 있다.

```kotlin
val (hello, number) = Pair("Hello", 1234)
println(hello)							//Hello
println(number)							//1234
```



> Triple은 객체를 생성하고 3개의 객체를 넣을 수 있다.
>
> Pair와 사용방법은 동일하고 3번째 객체 참조는 `third` 를 이용

----

Pair, Thiple 사용방법 : https://codechacha.com/ko/kotlin-pair-and-triple/

---



## 확장 함수 (Extension Functions)

> 기존의 정의된 클래스에 함수를 추가하는 기능



자신이 만든 클래스는 새로운 함수를 쉽게 추가 할 수 있는데,

Standard Library 또는 다른 사람이 만든 라이브러리를 사용할 때 함수를 추가하기가 매우 어렵다.

코틀린에서는 위의 고충을 해결 할 수 있게 기존의 정의된 클래스에 함수를 추가 할 수 있다.



### 사용법

> fun 클래스이름.함수이름(인자타임): 리턴타입 {구현부}

ex) List 클래스에 `getHigherThan()` 함수 추가

```kotlin
fun List<Int>.getHigherThan(num : Int) : List<int>{
  val result = arrayListOf<int>()
  for (item in this){
    if (item > num) {
      result.add(item)
    }
  }
  return result
}

fun main(){
  val numbers : List<Int> = listOf(1,2,3,4,5,6)
  val filtered = numbers.getHigherThan(3).toString()
  println(filtered)
}
```

----

확장 함수 사용방법 : https://codechacha.com/ko/kotlin-extension-functions/



## Keyword

### external

> C++로 작성된 함수를 실행 할 때의 키워드

Java에서는 JNI (Java Native Interface)를 이용하여 C++로 작성된 함수를 실행할 때 `native` 키워드를 이용해서 함수를 선언해준다.

Kotlin에서는 `external` 키워드를 이용한다.

`public external fun Hello()`



### invoke

> 이름 없이 간편하게 호출될 수 있는 함수

코틀린에는 `invoke` 라는 특별한 함수, 정확히는 연산자가 존재한다.

`invoke` 연산자는 이름 없이 호출될 수 있다.

이름 없이 호출된다는 의미는 아래 코드와 같다.

```kotlin
object MyFunction{
  operator fun invoke(str : String) : String{
    return str.toUpperCase()	// 모두 대문자로 바꿔줌
  }
}
```

`MyFunction` 이라는 오브젝트가 있다.

`object` 키워드로 만들었기에 `MyFunction` 은 하나의 객체처럼 사용될 수 있다.

즉, 하나의 객체이기 때문에 객체 안의 메서드를 호출하기 위해서 아래와 같이 호출하고 싶을 것이다.

```kotlin
MyFunction.invoke("hello")
```

물론 잘 동작하지만, kotlin에서 `invoke` 라는 이름으로 만들어진 함수는 특별한 힘을 갖는다.

이름 없이 실행 될 수 있는 힘이다.

즉, 아래와 같이 호출이 가능하다.

```kotlin
MyFunction("hello")
```



----

읽어 보면 좋은 사이트

1. 코틀린 스타일 가이드 : https://developer.android.com/kotlin/style-guide

2. 코틀린 키워드 연산자 : https://runebook.dev/ko/docs/kotlin/docs/reference/keyword-reference

   

## 새로운 내용 정리



### Spread Operator

> 배열을 단순 나열 할 때 사용 
>
> `*`

Spread : 퍼지다, 펼치다



코틀린의 Spread Operator `*` 는 배열을 단순 나열 할 때 사용한다.

ex)`*list` 의 의미 -> list의 요소들을 단순 나열하겠다. 라는 의미



```kotlin
val array = arrayOf("aa","bb","cc")

//val array2 = arrayOf(array[0], array[1], array[2], "dd", "cc")
val array2 = arrayOf(*array, "dd"" "cc")
```





----

참고사이트

Spread Operator : 티스토리 - https://seosh817.tistory.com/category/Android/Kotlin%28Java%29
