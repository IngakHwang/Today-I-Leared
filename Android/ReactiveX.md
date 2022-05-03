# ReactiveX

> 옵저버 패턴으로 구독자에게 변경사항을 알려주는 '비동기 이벤트 기반 프로그래밍'

Reactive Program은 옵저버 패턴으로 구독자에게 변경사항을 알려주는 '비동기 이벤트 기반 프로그래밍'

​	

Observer 패턴 + Iterator 패턴 + Functional 프로그래밍	 	

​	**Reactive (비동기 이벤트 방식) + X (이벤트 처리 방식)**



필요에 의해 Data 요청 -> Data 가공이 아닌, 데이터 관리주체 (Observable)에 데이터 변경 시 요청을 받겠다는 구독신청을 하고, 변경사항 (event) 발생 시 전달받는 방식

​	-> Reactive Programming은 하나의 값을 반환하기 보다 Data Stream을 반환한다.



Reactive (반응형) 프로그래밍과 Functional (함수형) 프로그래밍의 조합



**Reactive Programming?**

> 로직이나 데이터를 하나의 흐름 (Stream)으로 봄으로써 비동기 작업을 쉽게 다루는 것을 의미

| **기존 비동기 방식**                                         | **Reactive 비동기 방식**                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 비동기 작업 A를 완료하면 B를 호출(실행) (B는 Callback 또는 Interface 함수) | 비동기 작업 A가 이벤트를 발행하면 B가 구독을 수행 (B는 Observer 또는 Consumer) |



**Functional Programming?**

> 주어진 데이터에서 새로운 데이터를 반환 (과정을 알려주지 않음) 새로운 데이터를 반환하는 함수의 연속

| **명령형 프로그래밍 (절차형, 객체지향 .. 일반적인 방식)**    | **선언형 프로그래밍**                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 작업을 이렇게 저렇게 수행해라데이터 상태를 변경해서 결과를 반환 (알고리즘을 명시) | 특정 데이터가 되어라 (목적을 명시) 주어진 데이터에서 새로운 데이터를 반환하는 함수의 연속 |



## RxKotlin



`build.gradle`

```
implementation 'io.reactivex.rxjava2:rxkotlin:2.4.0'
```



### Observable

> 구독 대상자

- 이벤트 발행 주체 (Consumer 가 소비하는 값을 생성하는 역할)
- Observer 가 구독하는 대상 (subcribe() 으로 구독 등록)
- Observable Type (타입)
  - Observable : 최상위 기본 타입 (Default)
  - Single : 1개의 데이터만 반환 (null 반환 시 예외 발생)
  - Maybe : Null 가능성 있는 1개의 데이터 반환 (single + Nullable)
  - Completable : 반환값 없이 단순히 수행 후 종료 (Nullable)
  - Flowable : Backpressure 지원 Observable (발행/구독의 속도 차이 발생 시 속도 조절)



`예시`

```kotlin
Observable.just("One","Two")											// 1
		.subscribeOn(Scheduler.io())									// 2
		.observeOn (AndroidSchedulers.mainThread())		// 3
		.subscribe (																	// 4
      {value -> println("Result : $value")},			// 5
      {error -> println ("Error : $error")},			// 6
      {println("onComplete")}											// 7
    )
// Result : One
// Result : Two
// onComplete
```

```
1. Observable 선언, just() 는 내부 요소 순서대로 전달
2. Observable, Observer 실행 Thread 변경 (io 를 Worker 쓰레드, 백그라운드)
3. Observer 실행 Thread 변경 (UI 쓰레드)
4. 구독 등록, 3개 의 param
5. 데이터 발행 시 호출
6. 예외 발생 시 호출
7. 모든 데이터 발행 후 호출
```

Observable 생성 함수는 `create()` , `just()` 등 여러개가 존재한다.

`just()` 는 인자로 받은 데이터를 순서대로 발행(emit) 한다.

`subscribeOn` 은 선언 위치에 상관없이 Observable 과 Observer의 실행 Thread 를 설정하는 함수이다.

`observeOn` 선언 아래부분에만 선언한 Thread를 설정하는 함수

subscribeOn 보다 onserveOn의 우선순위가 높기 때문에 각각 Thread 영역이 다르게 설정

​	Observable - Io Thread (Worker Thread / Background Thread)

​	Observer - Main Thread (UI Thread)

`subscribe()` 는 구독자를 등록하는 함수로 Observer 인스턴스로 할당해도 되고 예시처럼 등록도 가능

