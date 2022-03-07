# Object 키워드

`object` 키워드를 object expression, object declaration 에서 사용 가능하다.



**한글 번역**

object expression = 객체 표현식

object declaration = 객체 선언



## object expression

> 객체 표현식
>
> `object` 키워드를 사용하여 익명 클래스의 객체를 생성할 때 사용
>
> `object` 키워드 뒤에 익명 클래스를 선언하면 그 시점에 바로 객체화



익명 클래스 : 클래스를 작성할 때 클래스 이름이 명시적으로 작성되어 있지 않은 클래스 (이름 없는 클래스)



예시

```kotlin
fun main(){
  val yourName = object{
    val lastName = "Hwang"
    val firstName = "Ingak"
    
    override fun toString() = "My name is $lastName $firstName"
  }
  
  println(yourName.toString())						// My name is Hwang Ingak
}
```

`object` 키워드 뒤에 나오는 코드를 보면 lastName, firstName 멤버 변수가 선언되고, toString() 멤버 메소드를 가지는 익명 클래스가 작성되어 있다.

위와 같이 익명 클래스를 작성하고 앞에 `object` 키워드를 붙여 그 시점에서 해당 익명 클래스의 객체가 바로 생성된다.



객체가 생성된다 => 해당 클래스의 생성자 함수 호출

`object` 키워드를 익명 클래스 앞에 붙여줌으로 익명 클래스 객체를 `object` 키워드 작성 시점에 바로 생성한다.

생선한 객체는 yourName 변수에 할당된다.



안드로이드 앱 개발 시 object expression 사용 예시

```kotlin
textView.setOnclickListener(object: View.OnClickListener{
  override fun onClick(v: View?){
    Log.d("log", "클릭")
  }
})
```

XX On XXX Listener() 메소드는 인자로 특정 인터페이스를 구현한 클래스의 객체를 넘겨줘야 하는 경우가 많은 데,

Object expressions을 사용해서 인자로 전달해야하는 객체를 생성

특정 인터페이스를 구현한 클래스를 더 이상 프로젝트 다른 곳에서 재사용하지 않기 떄문에 익명 클래스를 만들어 구현하는 것이다.



## object declaration

> 디자인 패턴 중 싱글톤 패턴을 만드는 작업을 할 때 사용
>
> 싱글톤 패턴 적용된 클래스 만들 때 사용하는 것



object declaration = 객체 선언



**싱글톤 패턴 (SingleTon)**

- 객체 지향 언어에서 필요한 클래스 들을 만들어놓고 실제로 사용할 때는 클래스를 토대로 만들어지는 객체라는 것을 생성해서 메모리에 할당한다.
- 이 때, 똑같은 클래스를 토대로 하여 만들어지는 객체를 이곳 저곳에서 여러번 생성한다면, 객체는 모두 따로따로 메모리 공간을 차지한다.
- 싱글톤 패턴으로 클래스를 만들어 놓으면 해당 클래스를 토대로 생성되는 객체는 여러 번 생성할 수 없다.
- 딱 한 번만 객체로 생성되어 메모리에 할당된다.
- 싱글톤 패턴이 적용된 클래스의 객체를 여러 곳에서 사용하더라도 이 객체는 항상 같은 메모리 주소를 바라본다.



**Java 싱글톤**

```java
public class MySingleTon{
  private static MySingleTon INSTANCE;
  
  private MySingleTon(){}
  
  public static MySingleTon getInstance(){
    if(INSTANCE == null){
      INSTANCE = new MySingleTon();
    }
    return INSTANCE
  }
  
  public void printName(){
    System.out.println("Myname is");
  }
}
```

- INSTANCE 라는 static 변수를 만들고 이 변수의 값을 null 체크하는 방식

- INSTANCE 가 null이 아니면(한 번이라도 객체가 생성된 적 있다면) 기존의 생성되어 있던 객체를 반환해줌으로써 새로운 객체가 여러 번 생성되는 것을 막는 방식

- Thread-safe 하지 않은 방식이다.

  - 쓰레드 A 에서 위 코드 실행 하다가 `if(INSTANCE == null)` 코드 까지 실행 후 운영체제에 의해 쓰레드 B에게 제어권 넘어가서 위 코드를 실행한 후 다시 쓰레드 A가 위 코드 실행 하면 객체가 2개 생성된 것

  - Thread-safe 보장하기 위해서 코드를 추가해야 함 (`synchronized` 사용 )

    ```java
    public class Singleton{
      private Singleton(){}
      
      private static Singleton INSTANCE;
      
      public static Singleton getInstance(){
        if (INSTANCE == null){
          synchronized (Singleton.class){
            if(INSTANCE == null){
              INSTANCe = new Singleton();
            }
          }
          return INSTANCE;
        }
      }
    }
    ```



**kotlin 싱글톤**

```kotlin
object MySingleTon{
  fun printName(){
    println("Myname is")
  }
}
```

- `class` 키워드 대신 `object` 키워드를 붙여 클래스를 생성해주면 싱글톤 패턴이 적용된 클래스 생성
- 코틀린 언어 내부적으로 클래스 생성 시 thread-safe한 싱글톤 패턴이 되도록 처리 되어 있다.
- 이렇게 클래스 작성하면 작성 시점에 바로 해당 클래스의 객체를 단 한개 생성해 놓는다.





## companion object

> Object declaration을 일반 클래스 내부에서 바로 작성하는 것



companion = 동반자

`companion` 키워드를 object declaration 앞에 붙여준다.

```kotlin
class MyClass{
  companion object MySingleTon{
    fun printName(){
      println("MyNameis")
    }
  }
}
```

- MyClass 일반 클래스 내부에서 바로 싱글톤 패턴이 적용된 MySingleTon 클래스를 작성
- MyClass 안에 작성된 MySingleTon 객체는 작성 시점에서 바로 객체로 생성된다.
- 한 번만 생성되어 메모리에 한 번만 할당된다.
- `companion object` 이름은 생략 가능하다.



**변수 / 메소드 호출방법**

```kotlin
fun main(){
  val myName = MyClass.MySingleTon.printName()			// companion object 클래스의 이름 있을 시
  
  val myName = MyClass.printName()									// companion object 클래스의 이름 없을 시
}
```



### companion object vs java static



companion object와 java static이 비슷해 보이지만 완전히 똑같지는 않다.



자바 static 변수 / 메소드

- 싱글톤 패턴과 비슷한 개념
- 어떤 클래스의 객체 생성 시 객체가 생성되는 횟수에 상관 없이 static 으로 선언된 변수/메소드는 딱 한 번만 생성되어 메모리에 한 번만 할당된다.
- static 변수는 컴파일 시 메모리에 할당된다.



companion object가 static과 비슷한 이유

- `companion object` 로 선언된 클래스는 작성 시점에서 바로 객체화 되고, 한 번만 메모리에 할당되기 때문이다.
- 자바로 작성된 static 변수를 코틀린 언어로 변환시키면 companion object 안에 static 으로 선언된 변수들이 선언되어 있다.



차이점

둘의 변수와 메소드가 다르다.



Java static 변수 / 메소드

```java
public class Example{
  
  static int number = 0;
  
  public static Int getNumber(){
    return number
  }
 
}
```



Companion object

```kotlin
class MyClass{
  companion object Counter{
    
    private var count: Int = 0
    
    fun count(): Int{
      return count
    }
  }
}
```

MyClass 일반 클래스 내부에 Counter 라는 이름의 companion object가 선언되어 있다.



위 코드를 자바로 변환 하면

```java
public final class MyClass {
    private static int count;
    public static final MyClass.Counter Counter = new MyClass.Counter(); // Counter 클래스 생성

    // inner class로 작성되는 Counter 클래스
    public static final class Counter {
        // 첫 번째, 생성자 함수 = private
        private Counter() {}
          
        public final void count() {
            return MyClass.count;
        }
          
        // 두 번째 생성자 함수 = public
        public Counter(DefaultConstructorMarker d) {
            this(); // = Counter();
        }
    }
}
```

- Companion object로 선언된 Counter 클래스는 자바에서는 사실 MyClass의 inner class로 작성되었다.
- Counter 클래스의 생성자 함수가 private, public 두 가지로 생성되어 있다.
- MyClass에서 Counter 클래스의 public 생성자 함수를 호출해 Counter 클래스의 객체를 생성하는 코드가 있다.
- Counter 클래스의 객체는 static final 변수에 할당된다.
- Counter 클래스의 객체는 MyClass 클래스의 객체가 생성될 때 자동으로 생성되는 객체 이다.



static 메소드 일 것 같았던 count() 메소드는 Counter 클래스의 멤버 메소드이기 때문에 static과 다르다.



static 메소드와 정말 똑같은 변환

멤버 메소드에는 `@JvmStatic`

멤버 변수에는 `@JvmField`  어노테이션을 붙이면 JVM이 companion object로 선언한 클래스의 멤버 변수/메소드를 static 변수/ 메소드 처럼 변환해준다.

즉, companion object 로 선언한 클래스의 멤버 변수/메소드를 inner class로 만들지 않고 static 변수/메소드와 같은 모습으로 변환한다.

```kotlin
class A {
  companion object {
    @JvmStatic
    fun count(){
      
    }
    
    @JvmField
    val number = 0
  }
}
```



----

참고 사이트

김초희 깃허브 : https://choheeis.github.io/newblog//articles/2021-07/kotlin-object-keyword