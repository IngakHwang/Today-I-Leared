# Annotation

> 코드 사이에 주석처럼 쓰이며 특별한 의미, 기능을 수행하도록 하는 기술



사전적 의미로는 주석이라는 뜻

코드 사이에 주석처럼 쓰이며 특별한 의미, 기능을 수행하도록 하는 기술이다.

특정 코드에 달아 어떤 의미를 부여하거나 기능을 주입하는 기술



- 프로그램에게 추가적인 정보를 제공해주는 메타데이터
  - 메타데이터 (meta data) : 데이터를 위한 데이터



## 어노테이션 용도

- 컴파일러에게 코드 작성 문법 에러를 체크하도록 정보를 제공
- 소프트웨어 개발툴이 빌드나 배치시 코드를 자동으로 생성할 수 있도록 정보 제공
- 실행시(런타임 시) 특정 기능을 실행하도록 정보를 제공



## 어노테이션 사용

1. 어노테이션 정의
2. 클래스에 어노테이션 배치
3. 코드 실행되는 중 Reflection 을 이용하여 추가 정보를 획득하여 기능 실행



### Reflection

> 프로그램이 실행 중에 자신의 구조와 동작을 검사하고, 조사하고, 수정하는 것

- Reflection이란 프로그램이 실행 중에 자신의 구조와 동작을 검사하고, 조사하고, 수정하는 것이다.

- Reflection은 개발자가 데이터를 보여주고, 다른 포맷의 데이터를 처리하고, 통신을 위해 serializtion(직렬화)를 수행하고, bundling을 하기 위해 일반 소프트웨어 라이브러리를 만들도록 도와준다.

- 객체지향 프로그래밍 언어에서 Reflection을 사용하면 컴파일 타임에 인터페이스, 필드, 메소드의 이름을 알지 못해도 실행 중에 클래스, 인터페이스, 필드 및 메소드에 접근 할 수 있다.

- 새로운 객체의 인스턴스화 및 메소드 호출을 허용한다.

- 객체 지향 프로그래밍 언어에서 Reflection을 사용하여 멤버 접근 가능성 규칙을 무시할 수 있다.

- Annotation 자체는 아무런 동작을 가지지 않는 단순한 표식일 뿐이지만, Reflection을 이용하면 Annotation의 적용 여부와 엘리먼트 값을 읽고 처리할 수 있다.

- Class에 적용된 Annotation 정보를 읽으려면 java.lang.Class를 이용하고
  필드, 생성자, 메소드에 적용된 어노테이션 정보를 읽으려면 Class의 메소드를 통해 java.lang.reflect 패키지의 배열을 얻어야 한다.
  \- Class.forName(), getName(), getModifier(), getFields() getPackage() 등등 여러 메소드로 정보를 얻을 수 있다.

- Reflection을 이용하면 Annotation 지정만으로도 원하는 클래스를 주입할 수 있다.



## 어노테이션 종류

### `@Override`

선언한 메서드가 오버라이드 되었다는 것을 나타낸다.

만약 상위 클래스(또는 인터페이스) 에서 해당 메서드를 찾을 수 없다면 컴파일 에러를 발생시킨다.



### `@Deprecated`

해당 메서드가 더 이상 사용되지 않음을 표시

만약 사용할 경우 컴파일 경고를 발생



### `@SuppressWarnings`

선언한 곳의 컴파일 경고를 무시



### `@SafeVarargs`

제너릭 같은 가변인자의 매개변수를 사용할 때의 경고를 무시



### `@FunctionalInterface`

함수형 인터페이스를 지정하는 어노테이션

만약 메서드가 존재하지 않거나, 1개 이상의 메서드(default 메서드 제외)가 존재할 경우 컴파일 오류 발생



### `@Retention`

컴파일러가 어노테이션을 다루는 방법을 기술하며, 특정 시점까지 영향을 미치는지를 결정

- `RetentionPolicy.SOURCE` : 컴파일 전까지만 유효 (컴파일 이후 사라짐)
- `RetentionPolicy.CLASS` : 컴파일러가 클래스를 참조할 때까지 유효
- `RetentionPolicy.RUNTIME` : 컴파일 이후에도 JVM에 의해 계속 참조가 가능 (리플렉션 사용)



### `@Target`

어노테이션이 적용할 위치 선택

종류는 다음과 같다.

#### `Java`

- `ElementType.PACKAGE` : 패키지 선언
- `ElementType.TYPE` : 타입 선언
- `ElementType.ANNOTATION_TYPE` : 어노테이션 타입 선언
- `ElementType.CONSTRUCTOR` : 생성자 선언
- `ElementType.FILED` : 멤버 변수 선언
- `ElementType.LOCAL_VARIABLE` : 지연 변수 선언
- `ElementType.METHOD` : 메서드 선언
- `ElementType.PARAMETER` : 전달인자 선언
- `ElementType.TYPE_PARAMETER` : 전달인자타입 선언
- `ElementType.TYPE_USE` : 타입 선언



#### `Kotlin`

- `AnnotationTarget.CLASS` : 클래스, 인터페이스, 객체, 어노테이션에 선언
- `AnnotationTarget.ANNOTATION_CLASS` : 어노테이션 클래스만 선언
- `AnnotationTarget.TYPE_PARAMETER` : 전달인자타입 선언
- `AnnotationTarget.PROPERTY` : 프로퍼티 선언
- `AnnotationTarget.FIELD` : 멤버 변수 선언
- `AnnotationTarget.LOCAL_VARIABLE` : 지역 변수 선언
- `AnnotationTarget.VALUE_PARAMETER` : 함수 매개변수 또는 생성자 매개변수 선언
- `AnnotationTarget.CONSTRUCTOR` : 생성자 선언
- `AnnotationTarget.FUNCTION` : 함수 선언 (생성자 아님)
- `AnnotationTarget.PROPERTY_GETTER` : 프로퍼티 게터 선언
- `AnnotationTarget.PROPERTY_SETTER` : 프로퍼티 세터 선언
- `AnnotationTarget.TYPE` : 
- `AnnotationTarget.EXPRESSION`
- `AnnotationTarget.FILE` : 
- `AnnotationTarget.TYPEALIAS` : 



### `@Inherited`

어노테이션의 상속을 가능하게 한다.



### `@Repeatable `

연속적으로 어노테이션을 선언할 수 있게 해준다.



### `@ToJson`



### `@FromJson`



### `@JsonQualifier`



----

참고 사이트



velog : https://velog.io/@ruinak_4127/Annotation%EC%9D%B4%EB%9E%80

강남언니 공식 블로그 : https://blog.gangnamunni.com/post/kotlin-annotation

티스토리 : https://bangu4.tistory.com/199