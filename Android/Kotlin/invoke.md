# invoke

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



`MyFunction` 은 객체다.

그렇기 때문에 `MyFunction` 을 `print` 해보면 `MyFunction` 의 주소값만 출력될 뿐이다.

그런데 `MyFunction` 안에 `invoke()` 함수가 정의 되어 있으므로 `MyFunction` 에서 메서드 이름 없이 바로 호출한 것이다.

물론 파라미터를 받을 창구가 있어야 하므로 () 안에 파라미터를 넣어서 실행이 가능하다.



## 연산자

이렇듯 분명히 `invoke` 와 같이 이름을 부여한 함수임에도 불구하고 실행을 간편하게 할 수 있게 하는 것들을 연산자라고 부른다.

그런 연산자들 몇 개를 코틀린에서 미리 정해놓았다.

대표적으로 + 연산자가 있다.

```kotlin
object Sample{
  operator fun plus(str : String) : Stirng{
    return this.toString() + str
  }
}

main() {
  Sample + " Hello~!" // [Sample 주소값] Hello~!
}
```

실행해보면 `[Sample의 주소값]` 부분에는 실제로 주소값이 들어가고, 그 뒤에 Hello~! 라는 글자가 붙을 것이다.

즉, `plus` 라는 이름으로 함수를 만들었지만 `plus` 는 코틀린에서 연산자로 정의해 놓았으므로 `plus` 연산자를 호출하기 위해 특별히 `+` 기호를 사용해서 호출한다.



## 람다는 invoke 함수를 가진 객체

코틀린은 람다를 지원한다.

이미 코틀린이 기본으로 제공하는 함수를 한 번 감싸는 의미 없는 코드이지만 이해를 위해 작성

```kotlin
val toUpperCase = {str : String -> str.toUpperCase()}
```

모든 값, 또는 값을 담는 변수에는 타입이 있다.

Int 일수도, String 일수도 또는 새로 만든 클래스 타입일 수도 있다.

그렇다면 위의 `toUpperCase` 는 무슨 타입일까?

Int, String 둘다 아니다.

`String` 을 받고, 다시 `String` 을 반환하는 (String) -> String 타입이다.



그러나 (String) -> String은 좀 생소하다.

정확히 어떤 의미일까?

위 타입은 코틀린 표준 라이브러리에 정의된 `Function1<P1, R>` 인터페이스 타입이다.

`Function<P1, R>` 의 구현을 살펴보면 `invoke(P1) : R` 연산자 하나만 달랑 존재한다.

위에서 언급했든이 `invoke` 연산자는 이름 없이 호출할 수 있다.

이름 없는 함수인 람다와 연관성이 있다.



결국 위에서 작성한 `toUpperCase` 는 아래 코드와 같다.

```kotlin
val toUpperCase = object : Function1<String, String>{
  override fun invoke(p1 : String) : String{
    return p1.toUpperCase()
  }
}
```

실제로 코틀린에서 작성한 람다는 위의 코드와 같기 때문에 결국 람다도 컴파일 시간에 람다가 할당된 객체로 변환된다는 뜻이다.

개발자는 그것의 invoke를 호출하는 셈이다.



`toUpperCase` 는 현재 `invoke` 라는 연산자 하나를 가진 객체이다.

`toUpperCase.invoke("hello")` 가 아닌 `toUpperCase("hello")` 와 같이 호출할 수 있음을 기억하자.

```kotlin
fun main(){
  val strList = listOf("a","b","c")
  println(strList.map(toUpperCase))		// [A,B,C]
}
```

`map` 함수는 `strList` 의 요소들을 순회하면서 각각의 요소 마다 `toUpperCase(요소)` 를 실행할 것이다.

`toUpperCase` 가 `invoke` 연산자를 가지고 있기 때문에 이렇게 편하게 사용 가능한 것이다.

또한 위의 코드는 아래와 같이 작성할 수 있다.

```kotlin
fun main(){
  val strList = listOf("a","b","c")
  println(strList.map{str: String -> str.toUpperCase()})
}
```

`{str : String -> str.toUpperCase()}` 이 부분이 결국 런타임 시 `invoke` 를 하나 가지는 오브젝트로 변환된다.



----

참고사이트

쾌락코딩 깃험 : https://wooooooak.github.io/kotlin/2019/03/21/kotlin_invoke/