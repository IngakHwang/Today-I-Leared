# Nullable, Non-nullable

> 코틀린은 자바와 다르게 Nullable, Non-nullalbe 타입으로 프로퍼티를 선언 할 수 있다.
>
> Non-nullable 타입으로 선언 시 객체가 null 이 아닌것을 보장하기 때문에 null check 코드 작성을 할 필요가 없다.



>  코틀린은 null에 안전하도록 설계되어 있다.



- `?`

  > 타입 뒤에 `?` 있으면 ⇒ nullable(null 할당 가능)
  >
  > 없으면 non-Nullable (null 할당 불가능)

  ```kotlin
  var nullable: String? = "nullable"        //nullable
  var nonNullable: String = "non-Nullable"  //non-Nullable
  ```

- `!!`

  > null이 아니라는 것을 보장할 수 있는 객체에만 사용
  >
  > 메서드 뒤에 사용

  ```kotlin
  // nonNullString 은 Non-nullable
  // getString() 은 nullable
  
  //컴파일 에러
  //String?타입을 String에 할당하려고 했기 때문
  var nonNullString: String = getString()  
  
  //컴파일 성공
  var nonNullString: String = getString()!!
  ```

- `?.`

  > Safe call 연산자 (안전하게 nullable 프로퍼티 접근)
  >
  > if 프로퍼티가 null ~~

  ```kotlin
  val a: String = "Kotlin"
  val b: String? = null
  println(b?.length)      //b가 null 이라면 length를 호출하지 않고 null 리턴
  println(a?.length)
  ```

- `?:`

  > 엘비스 연산자 (Elvis Operation)
  >
  > 삼항연산자와 비슷

  ```kotlin
  //null이 아니라면 ?: 왼쪽 객체 리턴
  //null이라면 ?:의 오른쪽 객체 리턴
  val 1 = b?.length ?: -1
  ```



----

참고 사이트

코드차차 : https://codechacha.com/ko/kotlin-null-safety/

코틀린 공홈 : https://kotlinlang.org/docs/null-safety.html#collections-of-a-nullable-type 