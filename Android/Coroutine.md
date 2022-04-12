# Coroutines

> 코루틴은 스레드 안에서 실행되는 일시 중단 가능한 작업의 단위이다. 
>
> 하나의 스레드에 여러 코루틴이 존재할 수 있다.



코루틴은 코틀린만의 것이 아니다.

이름이 비슷해서 코틀린의 것이라고 생각할 수 있지만 파이썬, C#, Go, Javascript 등 여러 언어에서 지원하고 있는 개념이다.

앱이든 웹이든 비동기 처리가 핵심인 클라이언트 프로그래밍에서 지금까지 가장 핫한 키워드는 RX Programming 이다.

그러나 구글이 안드로이드 공식 언어를 자바에서 코틀린으로 변경 이후 샘플 예제들인 bluprint, sunflower 앱의 비동기처리를 coroutine 으로 바꾸었다.

코루틴을 사용하면 비동기 처리가 너무나도 쉽게 이루어질 수 있기 때문이다.



## 왜 Coroutine이 필요할까?

> 스레드 작업을 최적화
>
> Blocking 되는 상황이 줄어 Thread의 리소스를 최대한 활용할 수 있다.
>
> Thread는 만드는 비용이 큰데, Coroutine은 Thread를 만드는 대신 하나의 Thread 상에서 자신을 일시 중지할 수 있도록 하여 Thread 생성 비용을 줄인다.



### 기존 JVM 프로세스

하나의 프로세스에는 여러 스레드가 있고, 각 스레드는 독립적으로 작업을 수행할 수 있다.

예를 들어 JVM 프로세스 상에서는 스레드는 아래 그림과 같이 구성된다.

![image-20220411183639023](https://tva1.sinaimg.cn/large/e6c9d24egy1h15x273ytkj20s60to77j.jpg)

JVM 프로세스는 Main Thread가 종료되면 강제로 종료되며, JVM 프로세스에 속상 Thread들도 함께 강제로 종료된다.

하지만 Main Thread 말고 다른 2개의 Thread 들은 Main Thread와 마찬가지로 작업을 수행할 수 있지만, 이 Thread들은 종료되더라도 다른 Thread에 영향을 미치지 않는다.



안드로이드에서 Main Thread는 가장 중요한 UI Thread 역할로 UI를 그려주고 사용자가 화면터치 이벤트를 전달받는 스레드이다. 사용자와 인터렉션을 담당하는 Thread이다.

만약 이 Thread가 높은 부하를 받는 작업에 의해 블로킹된다면 안드로이드 앱은 멈춤 현상이 일어나고, 일정 시간 이상 블로킹 될 때 앱은 강제로 종료된다.

따라서 Main Thread에서 많은 부하를 받는 작업은 지양해야 하며, 다른 Thread를 생성해 해당 Thread에 높은 부하를 주는 작업을 수행하도록 만들어야 한다.



### 기존 접근 방식과 한계점

위의 문제를 해결하기 위해서 여러가지 방법이 시도 되었다.

1. Runnable 인터페이스를 구현하는 클래스를 만든 다음 Thread에 해당 클래스를 넣어 start

   ```kotlin
   fun main(){
     Thread(ExampleRunnable()).start()
     Thread(ExampleRunnable()).start()
   }
   
   class ExampleRunnable : Runnable{
     override fun run(){// code }
       
     
   }
   ```

2. ExecuterService를 이용해 Thread pool 구성하여 작업 진행

   ```kotlin
   fun main(){
     val executerService = Executors.newFixedThreadPool(4)
     
     executerService.submit(ExampleRunnable())
     executerService.submit(ExampleRunnable())
   }
   
   class ExampleRunnable : Runnable{
     override fun run(){// code }
   ```

3. Rx 라이브러리를 이용하는 방식

   ```kotlin
   publisher.subscribeOn(Schedulers.io())
   				.observeOn(AndroidSchedulers.mainThread())
   ```

   Rx 라이브러리는 Reactive Programming을 돕기 위한 라이브러리이다.

   데이터를 발행하는 Thread와 데이터를 구독하는 Thread를 손쉽게 제어할 수 있게 함으로써 스레드 작업을 쉽게 하기 위해 도운다.



위와 같은 기존 접근 방식들의 한계점은 작업의 단위가 Thread 라는 점이다.

다른 Thread로 작업을 넘기면 된다고 했는데 작업의 단위가 Thread인 것이 무슨 말이고, 무슨 문제일까?



Thread는 생성 비용이 비싸고 작업을 전환하는 비용이 비싸다.

또한 한 Thread가 다른 Thread 부터의 작업을 기다려야 할 때 Blocking 되게 되면 해당 Thread는 하는 작업 없이 다른 작업이 끝마쳐질 때 까지 기다려야 하기 때문에 자원은 낭비된다.

위의 작업 단위가 Thread일 경우 생기는 고질적인 문제점이다.



![image-20220411185038689](https://tva1.sinaimg.cn/large/e6c9d24egy1h15xgrfdavj21ho0i275o.jpg)



위 그림을 보면 Thread1에서 작업1 수행 도중 Thread2의 작업2의 결과물이 작업1을 수행하는 데 필요하다.

그 때 Thread1은 아무것도 하는 일 없이 Blocking 되면 Thread2로 부터 결과를 전달받아 작업1을 재개하기 까지 많은 시간이 소요된다.

이렇게 짧은 시간동안만 Blocking되면 다행이지만, 실제 상황에서는 Thread의 성능을 반도 발휘하지 못하게 만드는 Blocking이 반복될 수 있다.



### Coroutine이 기존 한계점을 극복하는 법

코루틴에서도 Thread 라는 작업의 단위를 사용하지만, Thread 내부에서 작은 Thread처럼 동작하는 코루틴이 존재한다.

Thread 하나를 일시중단 가능한 다중 경량 Thread 처럼 활용하는 것이 바로 Coroutine 이다.

![image-20220411185534036](https://tva1.sinaimg.cn/large/e6c9d24egy1h15xlv2hroj20e40iewez.jpg)

경량 Thread가 무엇일까? 일시 중단 가능한 것이 무엇일까?

![image-20220411185612065](https://tva1.sinaimg.cn/large/e6c9d24egy1h15xmj80xuj21ha0iq766.jpg)

1. Thread1 에서 Coroutine2개를 생성한다.

   Thread1의 Coroutine1 에서는 작업1을 그대로 수행하게 만들고

   Thread2에서는 작업2를 수행하게 만든다.

   이 때 마찬가지로 Thread1의 Coroutine1에서 작업1 수행 도중 Thread2로 부터 결과가 필요해진다.

   하지만 Thread2의 작업2가 끝나지 않아 작업1을 마저 할 수 없다.

2. 이 때 Coroutine1은 Thread1을 Blocking 하는 대신 자신의 작업을 일시중단하고 Coroutine2에 Thread1 리소스 사용 권한을 남겨준다.

   Thread1의 Coroutine2가 작업3을 위해 Thread1을 사용한다.

3. 이 후 Thread2의 작업이 종료되고, 작업3을 수행하던 Coroutine2이 자신의 작업을 마무리하고 Coroutine2 자신을 일시중단 시킨다.

   다시 Thread1의 제어권한이 Coroutine1로 돌아오고, Thread1은 Coroutine1이 마저 작업을 수행할 수 있도록 Thread2로 부터 결과를 전달받아 작업1을 재개한다.



**Blocking 되는 상황이 줄어 Thread의 리소스를 최대한 활용할 수 있다.**

**Thread는 만드는 비용이 큰데, Coroutine은 Thread를 만드는 대신 하나의 Thread 상에서 자신을 일시 중지할 수 있도록 하여 Thread 생성 비용을 줄인다.**





## Coroutine 이해

> 경량 스레드

코루틴을 3가지 키워드 정도로 알아 볼 수 있다.

1. 협력형 멀티 태스킹
2. 동시성 프로그래밍 지원
3. 비동기 처리를 쉽게 도와줌



가장 중요한 개념은 1번, 협력형 멀티 태스킹 이다.

협력형 멀티태스킹에 대한 내용을 이해한다면 코루틴 이란 것을 알게되는 것이다.

그러나 코루틴을 내 것으로 만들기 위해서는 동시성 프로그래밍과, 비동기 처리에 대한 관점에서 이해하는 것도 중요하다.



### 협력형 멀티 태스킹

협력형 멀티태스킹을 프로그래밍 언어로 표현하자만 Co + Routine 이다.

Co 라는 접두어는 "협력", "함께" 라는 의미를 지니고 있다.

Routine은 하나의 태스크, 함수 정도로 생각하면 된다.

즉 협력하는 함수다.



좀 더 나아가 Routine에 대해서 좀 더 알아보자

Routine 에는 우리가 흔히 알고 있는 main routine, sub routine이 존재한다.

이런 단어들이 생소 할 수도 있지만, 우리가 늘 작성하는 있는 코드들이다.



```kotlin
// Main routine
fun main(){
  ...
  val addedValue = plusOne(10)
}

// Sub routine
fun plusOne(value : Int) : Int {
  val one = 1
  val addedValue = value + one
  
  return addedValue
}
```

위의 코드를 보면 `main` 함수가 말 그대로 Main 함수이다.

메인이 되는 함수인 것이다.

그리고 메인이 되는 함수는 다른 서브 함수인 `plusOne` 을 호출한다.

우리가 짜는 프로그램은 흔히 이렇게 되어있다.



그런데 이 Sub Routine을 살펴보면 한가지 특징이 있다.

![image](https://user-images.githubusercontent.com/18481078/63651648-f8ced280-c791-11e9-9917-1b034b855e84.png)

Sub Routine은 루틴에 진입하는 지점과 루틴을 빠져나오는 지점이 명확하다.

즉, 메인 루틴이 서브루틴을 호출하면, 서브루틴의 맨 처음 부분에 진입하여 `return` 을 만나거나 서브루틴의 닫는 괄호를 만나면 해당 서브루틴을 빠져나오게 된다.



```kotlin
// Main routine
fun main(){
  ...
  val addedValue = plusOne(10)
}

// Sub routine

fun plusOne(value : Int) : Int {
/*서브루틴 진입*/
  val one = 1
  val addedValue = value + one
  
  return addedValue
/*서브루틴 탈출*/
}
```

메인 쓰레드가 `plusOne` 이라는 서브루틴에 진입한다.

당연히 코드는 처음부터 진입이 되어 맨 윗줄부터 실행이 될 것이고, 그 아래 코드들을 쭉 실행 후 `return` 을 만나면 서브루틴을 호출했던 부분으로 탈출한다.

그리고 진입전과 탈출점 사이에 쓰레드는 블락되어 있다.



그러나 코루틴 (Coroutine)은 조금 다르다.

![image](https://user-images.githubusercontent.com/18481078/63651705-a0e49b80-c792-11e9-9924-eb737b813065.png)

코루틴도 routine이기 때문에 하나의 함수로 생각하자.

그런데 이 함수에 진입할 수 있는 진입점도 여러개이고,

함수를 빠져나갈 수 있는 탈춤절도 여러개이다.

즉, 코루틴 함수는 꼭 `return` 문이나 마지막 닫는 괄호를 만나지 않더라도 언제든지 중간에 나갈 수 있고, 언제든지 다시 나갔던 그 지점으로 들어올 수 있다.

```kotlin
fun drawPerson(){
  startCoroutine{
    drawHead()
    drawBody()
    drawLegs()
  }
}

suspend fun drawHead(){
  delay(2000)
  ...
}

suspend fun drawBody(){
  delay(2000)
  ...
}

suspend fun drawLegs(){
  delay(2000)
  ...
}

/*
1. 
쓰레드의 main함수가 drawPerson()을 호출하면 하나의 코루틴 블럭(함수)이 생성이 된다.
drawPerson()은 언제든 진입, 탈출 할 수 있는 자격이 주어진다.

2.
코투린 함수가 실행되는 과정에서 suspend 키워드를 가진 함수를 만나게 되면,
더 이상 아래 코드를 실행하지 않고 멈추고(suspend) 코루틴 block을 탈출한다.

3.
메인 쓰데르의 다른 코드들이 실행된다.
그러나 Head는 어디선가 계속 그려지고 있다.

4.
다른 코드들이 실행되다가도, drawHead가 끝나면
다시 코루틴으로 진입해서 아까 멈춘 부분(drawHead) 아래부터 다시 실행된다.
*/

```



`drawPerson` 이라는 함수가 있다.

이 함수 안에는 `startCoroutine` 이라는 코루틴 빌더가 있다. 

(실제로 startCoroutine 이라는 빌더는 존재하지 않는다. 실제 코루틴 라이브러리에는 다른 방식으로 코루틴을 만들지만 이해하기 쉽게 startCoroutine 이라고 사용한다.)



`startCoroutine` 이라는 코루틴을 만나게 되면 해당 함수는 코루틴으로 작동할 수 있다.

따라서 언제든 함수 실행 중간에 나갈 수도 있고,

다시 들어올 수도 있는 자격이 부여되는 것이다.



언제 코루틴을 중간에 나갈 수 있을까?

`suspend` 로 선언된 함수를 만나면 코루틴 밖으로 잠시 나갈 수 있다.



**순서**

1. 쓰레드의 main() 함수가 `drawPerson()` 을 호출하면 `startCoroutine` 블럭을 만나 하나의 코루틴을 만들어 시작한다. 이제 `drawPerson()` 은 진입점과 탈출점이 여러개가 되는 자격이 주어진 것이다.
2. 코루틴이 실행이 되었지만, `suspend` 를 만나기 전까지는 그다지 특별한 힘이 없다. `suspend` 로 정의된 함수가 없다면 그냥 마지막 괄호를 만날 때까지 계속 실행된다. 그러나 `drawHead()` 는 `suspend` 키워드로 정의되어진 함수다. 따라서 `drawHead()` 부분에서 더 이상 아래 코드를 실행하지 않고 `drawPerson()` 이라는 코루틴 함수를 (잠시) 탈출한다.
3. 메인 쓰레드가 해당 코루틴을 탈출하면, 쓰레드가 놀고 있을리는 없다. 우리가 짜 놓은 다른 코드들을 실행할 수도 있고, 안드로이드라면 UI 애니메이션 처리 할 수도 있다. **그러나 Head는 어디선가 계속 그려지고 있다.** `drawHead()` 는 2초가 걸리는 `suspend` 함수이다. `drawHead()` 라는 `suspend` 를 만나 코루틴을 탈출했지만, **`drawHead()` 함수의 기능은 메인쓰레드에서 동시성 프로그래밍으로 작동하고 있을 수도 있고, 다른 쓰레드에서 돌아가고 있을 수도 있다. 그것은 개발자가 자유롭게 선택할 수 있다**
4. 그렇게 메인쓰레드가 다른 코드들을 실행하다가, `drawHead()` 가 제 역할을 다 끝내면 다시 아까 탈출했던 코루틴 `drawPerson()` 으로 돌아온다. 아까 멈추어놓았던 `drawHead()` 아래인 `drawBody()` 부터 재개 된다.



코루틴 함수는 언제든지 나왔다가 다시 들어올 수 있다.

코루틴의 이런 성향은 동시성 프로그래밍과 밀접한 관계가 있다.



### 동시성 프로그래밍



함수를 중간에 빠져나왔다가, 다른 함수에 진입하고, 다시 원점으로 돌아와 멈추웠던 부분부터 다시 시작하는 이 특성은 동시성 프로그래밍을 가능하게 한다.

동시성 프로그래밍의 개념을 잡고 가자면, 병렬성 프로그래밍과 완전히 다른 개념이다.

예를 들어 양쪽에 놓여진 2개의 도화지에 사람 그림을 각자 그린다고 가정해보자.

![image](https://user-images.githubusercontent.com/18481078/63692638-1dd44b80-c84d-11e9-952f-c78a58bec3e0.png)

동시성 프로그래밍이란

오른쪽 손에만 펜을 쥐고서 왼쪽 도화지에 사람 일부를 조금 그리고,

오른쪽 도화지에 갖서 잠시 또 사람을 그리고,

다시 왼쪽 도화지에 사람을 찔끔 그리는 것을 반복

이 행위를 아주 빨리 반복하는 것이다.



사실 내가 쥔 펜은 한 순간에 하나의 도화지에만 닿는다.

그러나 이 행위를 멀리서 본다면 마치 동시에 그림이 그려지고 있는 것처럼 보일 것이다.

이것이 동시성 프로그래밍이다.



병렬성 프로그래밍은 이것과 다르다.

병렬성은 실제로 양쪽 손에 펜을 하나씩 들고서 왼쪽, 오른쪽 동시에 그리는 것이다.

같은 시간 동안 2개의 그림을 그리는 것이다.



**코루틴은 개념자체로만 보면 병렬성이 아니라 동시성을 지원하는 개념이다.**

![image](https://user-images.githubusercontent.com/18481078/63692912-b9fe5280-c84d-11e9-9c8c-88b2bf5ade73.png)

코루틴도 루틴이다.

즉, 쓰레드가 아니라 일반 서브루틴과 비슷한 루틴이기 때문에 하나의 쓰레드에 여러개가 존재할 수 있다.



위의 코드에서는 메인 쓰레드에 코루틴이 2개가 있다.

하나는 왼쪽 도화지에 그림을 그리는 코드고

다른 하나는 오른쪽 도화지에 그림을 그리는 코드다.



메인 쓰레드가 실행되면서 먼저 왼쪽 코루틴인 `drawPersonToPaperA()` 라는 함수를 만났다고 가정해보자.

해당 함수는 가상 코루틴 빌더인 `startCoroutine{}` 블록으로 인해 코루틴이 되고, 함수를 중간에 나갔다가 다시 들어 올 수 있는 자격이 주어진다.

`drawPersonToPaperA()` 가 호출되어 `suspend` 함수인 `drawHead()` 를 만나게 되면 이 코루틴을 잠시 빠져나간다.



왼쪽 코루틴을 빠져나갔지만 메인쓰레드가 가만히 놀고 있진 않다.

다른 `suspend` 함수를 찾거나 resume되어지는 다른 코드들을 찾는다.

왼쪽 코루틴의 경우 2초 동안 `drawHead()`작업을 하게된다.

그러나 `delay(2000)` 는 쓰레드를 블락시키지 않으므로 다른 일들을 할 수가 있다.

뿐만 아니라 `drawHead()` 함수 안에서 다른 쓰레드를 실행시킨다면 병행적으로도 실행이 가능하다.



왼쪽 코루틴을 빠져나온 쓰레드가 오른쪽 코루틴을 만나게 되어 또 한번 `suspend` 함수를 만나게 되면 아까 도화지 그림에서 설명한 것과 같은 현상이 일어난다.

아까 오른속에 펜을 쥐고 왼쪽, 오른쪽 도화지를 아주 빠르게 왔다 갔다 하면서 그림을 그리는 것 같은 셈이다.

이렇게 코루틴을 사용하여 쓰레드 하나에서 동시성 프로그래밍이 가능한다.



코루틴을 생성해서 동시성 프로그래밍 하는 것과 쓰레드를 사용해서 동시성 프로그래밍을 하는 것과 차원이 다른 효율성을 제공한다.

![image](https://user-images.githubusercontent.com/18481078/63693725-c7b4d780-c84f-11e9-9158-0d6b65c614fe.png)

왼쪽 쓰레드는 왼쪽 도화지에,

오른쪽 쓰레드는 오른쪽 도화지에 그림을 그리는 쓰레드이다.

그러나 CPU는 단 한 개 뿐이다.

따라서 왼쪽에 조금, 오른쪽에 조금을 반복하기 위해서 CPU가 매번 쓰레드를 점유했다가 놓아주고, 새로운 쓰레드를 점유했다가 놓아주고를 반복해야 한다. 

이를 컨텍스트 스위칭이라고 한다.

하나의 쓰레드에서 단순히 함수를 왔다 갔다 하는 것과는 다르게 꽤 비용이 드는 작업이다.

![image](https://user-images.githubusercontent.com/18481078/63693907-2d08c880-c850-11e9-8160-9198c9a755d1.png)



### 편한 비동기 처리

위에서 설명한 코틀린의 능력으로 비동기 처리가 굉장히 쉬워진다.



예시) 기상 부터 회사에 도착하기 까지 과정

1. 기상
2. 샤워
3. 옷 입기
4. 출발
5. 도착
6. 일하기

위의 시나리오는 꼭 순서대로 이뤄져야하는 작업이다.

그리고 각 과정은 시간이 오래 걸리는 작업이라고 가정한다.



**callback**

```kotlin
fun goCompany(person : Person){
  val 주인공 = person
  
  wakeUp(주인공) {일어난주인공 ->
  	takeShower(일어난주인공){ 씻은주인공 ->
      putOnShirt(씻은주인공){ 옷입은주인공 ->
        getOnBus(옷입은주인공) -> { 버스탄주인공
          val 출근한주인공 = finish(버스탄주인공)
          출근한주인공.doWork()
        }
      }
    }
    
  }
}
```

콜백으로 처리 시 복잡해진다.



**RxKotlin**

```kotlin
fun goCompany(person : Person){
  val 주인공 = person
  
  Observable
  	.just(person)
  	.observeOn(MAIN_Thread)
  	.subscribeOn(IO_Thread)
  	.flatMap {주인공 -> wakeUp(주인공)}
  	.flatMap {일어난주인공 -> takeShower(일어난주인공)}
  	.flatMap {씻은주인공 -> putOnShirt(씻은주인공)}
  	.flatMap {옷입은주인공 -> getOnBus(옷입은주인공)}
  	.flatMap {버스탄주인공 -> finish(버스탄주인공)}
  	.subscribe({출근한주인공 -> 
    	출근한주인공.doWork()
    },{
      실패했을때 처리()
    })
}
```

콜백 코드보다 훨씬 눈에 잘 들어오는 것 같다.

함수들이 보통 사람이 생각하는 것처럼 순차적으로 보이기 때문일거 같다.

즉 각 과정의 함수들이 동일한 depth를 유지하며 동기적인 코드처럼보이기 때문에 훨씬 보기가 편해졌다.



그런데 Rx를 모르는 사람이 본다면 `Observable` 이 무엇인지, `just` 가 무엇인지, `flatMap` 이 무엇인지 모른다. 뿐만 아니라 Rx가 제공하는 operation들이 상당히 많기 때문에 학습 난이도가 있다.



**Kotlin + coroutine**

```kotlin
suspend goCompany(person : Person){
  val 주인공 = person
  try{
    val 일어난주인공 = wakeUp(주인공)
   	val 씻은주인공 = takeShower(일어난주인공)
    val 옷입은주인공 = putOnShirt(씻은주인공)
    val 버스탄주인공 = getOnBus(옷입은주인공)
    val 출근한주인공 = finish(버스탄주인공)
    
    출근한주인공.doWork()
    
  }catch (e : Exception){
    실패했을 때 처리()
  }
}
```



이 코드도 분명히 비동기 코드이다.

안에서 호출되는 각 함수들을 오래 걸리는 작업이고, 언제 끝날지 모르는 비동기 작업들이지만 각자 함수들의 순서는 정확히 지켜진다.



이게 가능한 이유는 `goCompany` 라는 함수가 코루틴이기에 `wakeUp` 을 만나면 `wakeUp` 함수를 실행함과 동시에 (여기서는 백그라운드 스레드에서 동시에 실행될 것이다.) 잠시 `goCompany` 를 빠져나간다.

그러다가 `wakeUp` 이 자신의 일을 끝마치면 다시 `goCompany` 로 돌아올 수 있기 때문이다.

이게 코루틴으로 비동기 처리를 할 때 생기는 장점이다.



### Dispatcher

> 코루틴을 만든 다음 해당 코루틴을 Dispatcher에 전송하면 Dispatcher는 자신이 관리하는 스레드풀 내의 스레드 부하 상황에 맞춰 코루틴을 배분

코루틴을 시작 시 Dispatcher란 단어가 가장 먼저 접하게 된다.

Dispatch 란 '보내다' 라는 뜻, Dispatcher는 무엇을 보내는 것인가?

바로 스레드(Thread)에 코루틴(Coroutines)을 보낸다.

코루틴에서는 스레드 풀을 만들고 Dispatcher를 통해서 코루틴을 배분한다.

즉, 코루틴을 만든 다음 해당 코루틴을 Dispatcher에 전송하면 Dispatcher는 자신이 관리하는 스레드풀 내의 스레드 부하 상황에 맞춰 코루틴을 배분한다.



**시작적으로 표현**

1. 유저가 코루틴을 생성한 후 Dispatcher에 전송한다.

   ![image-20220411212526743](https://tva1.sinaimg.cn/large/e6c9d24egy1h161xwi8pmj21eh0u040j.jpg)





2. Dispatcher는 자신이 잡고 있는 Thread Pool 에서 자원이 남는 스레드가 어떤 스레드인지 확인한 후, 해당 스레드에 코루틴을 전송

   <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h161zpmzeyj215m0u0wgk.jpg" alt="image-20220411212714933" style="zoom:40%;" />

3. 분배 받은 Thread는 해당 코루틴을 수행한다.

   <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h1621drtuqj20i20wwabf.jpg" alt="image-20220411212852015" style="zoom:40%;" />

#### 코루틴 스레드 풀

코루틴에서 스레드 풀을 만들기는 쉽다.

단순히 다음과 같은 코드를 수행하는 것만으로 스레드가 3개인 Dispatcher를 생성하는 것이 가능하다.

```kotlin
val dispatcher = newFixedThreadPoolContext(3, "ThreadPool")		// 스레드 3개인 Dispatcher
val singleDispatcher = newSingleThreadContext("SinglThreade")	// 스레드 1개인 Dispatcher
```



코루틴은 스레드 풀을 만들지만 직접 스레드 풀을 제어하지 않는다.

위 그림에서 볼 수 있듯이, 코루틴은 만들어진 스레드 풀을 직접 제어하지 않고 Dispatcher를 통해 제어한다.

즉, 스레드 풀을 직접 만들 수 있지만, 해당 스레드 풀 제어는 모두 Dispatcher에게 맡긴다.

개발자가 Dispatcher에게 코루틴을 보내기만 하면, Dispatcher는 스레드에 코루틴을 분산시킨다.



#### 안드로이드 Dispatcher

안드로이드에는 이미 Dispatcher가 생성되어 있어 별도로 생성하거나 정의할 필요가 없다.



**기본으로 생성되어 있는 Dispatcher**

- Dispatchers.Main - Android 메인 스레드에서 코루틴을 실행하는 디스패처, **UI와 상호작용하는 작업을 실행하기 위해서만 사용**
- Dispatchers.IO - **디스크 또는 네트워크 I/O 작업을 실행하는 데 최적화**되어 있는 디스패처
- Dispatchers.Default - **CPU를 많이 사용하는 작업을 기본 스레드 외부에서 실행하도록 최적화**되어 있는 디스패처, **정렬 작업이나 JSON 파싱 작업 등에 최적화**



### launch, async

> 결과 반환이 필요 없을 때 launch, 필요할 때 async
>
> 코루틴을 생성할 때 Dispatcher를 설정하는 것만으로 Dispatcher의 전환이 가능하다.



Dispathcer에 Coroutine을 붙이는 작업은 `launch{}` , `async{}` 두가지 메서드를 통해 가능하다.

결과 반환이 없는 단순 작업에는 launch

결과 반환이 있는 작업에는 async를 사용

|        | 결과 반환 | 반환타입   |
| ------ | --------- | ---------- |
| launch | X         | Job        |
| async  | O         | Defered<T> |



#### launch : 결과 반환X

launch는 결과를 반환하지 않고 launch 수행 시 Job이 반환된다.

```kotlin
with(CoroutineScope(Dispatchers.Main)){
  val job : Job = launch {println(1)}
}
```



#### async : 결과 반환O

async는 결과를 반환하며 결과값은 Deferred로 감싸서 반환된다.

Deffered는 미래에 올 수 있는 값을 담아놓을 수 있는 객체이다.

```kotlin
CoroutineScope(Dispatchers.Main).launch{
  val defferdInt : Defferd<Int> = async{
    printlnt(1)
    1							//마지막 줄 반환
  }
  val value = defferdInt.await()
  println(value)	// 1 출력
}
```

Async 블록의 마지막 줄에 있는 1이 반환되어야 하므로 `Defferred<Int>` 값이 반환되었다.



Deferred<T> 의 `await()` 메서드가 수행되면 코루틴은 결과가 반환되기 까지 기다린다.

이를 코루틴이 일시 중단 되었다고 한다.

이러한 특성으로 인해 **`await()` 메서드는 일시 중단이 가능한 코루틴 내부에서만 사용이 가능하다.**

만약 바깥에서 `await()` 를 사용하면 `fun` 키워드를 `suspend fun` (일시중단 함수) 로 만들라는 오류가 생긴다.

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h162s444szj21780qe433.jpg" alt="image-20220411215432889" style="zoom:40%;" />



위에서 두가지 방식을 통해 디스패처에 코루틴을 붙이는 기본적인 방법을 설명했다.

아래에서는 Dispatcher를 전환하면서 코루틴을 붙이도록하여 각 코루틴의 작업 성격에 맞게 Dispatcher를 설정하는 방법



#### Dispatcher 전환하면서 Coroutine 붙이기

파일 시스템으로부터 `Array<Int>` 를 받아와서 정렬한 다음 TextView에 출력하는 과정을 한다고 가정

이 과정에는 **파일 입출력 (Dispatchers.IO), Array 정렬 (Dispatchers.Default), TextView 출력 (Dispatchers.Main)과 같이 여러 Dispatcher에 맞는 작업이 들어가는 데 코루틴은 이러한 Dispatcher Switching을 하기 위한 간편한 방법을 제공**



```kotlin
// 1. Main Dispatcher를 기본으로 설정
CoroutineScope(Dispatchers.Main).launch{
  
  // 2. Data I/O 을 위한 IO Dispatcher에 배분
  val defferdInt : Deferred<Array<int>> = async(Dispatchers.IO){
    println(1)
    arrayOf(3, 1, 2, 4, 5)			// 마지막 줄 반환
  }
  
  // 3. Sort 해야 하므로 CPU 작업을 많이 해야하는 Default Dispatcher에 배분
  val sortedDefferd = async(Dispatchers.Default){
    val value = defferdInt.await()
    values.sortedBy{it}
  }
  
  // 4. 기본인 Main Dispatcher
  // TextView에 세팅하는 것은 UI 작업 = Main Dispatcher에 배분
  val textViewSettingJob = launch {
    val sortedArray = sortedDeferred.await()
    setTextView(sortedArray)
  }
}
```



1. 기본적으로 Main Dispatcher 안에서 시작되도록 `CoroutineScope(Dispatchers.Main).launch` 가 수행

2. 파일 시스템으로부터 [3, 1, 2, 4, 5] Array를 가져오는 작업은 결과를 반환 받아야 하는 작업이다.

   따라서 async를 사용하고, 이 때 **파일 I/O 이므로 전용 디스패처인 Dispathcers.IO 사용**

   아래와 같이 async 뒤에 Dispatchers.IO 를 붙임으로써 dispatcher 스위칭이 가능해진다.

   ```kotlin
   async(Dispathcers.IO){//데이터 입출력을 해야 하므로 IO Dispatcher에 배분
   	..
   }
   ```

3. **Sorting은 CPU작업을 많이 사용하므로 Dispatchers.Default에서 수행**

   결과를 반환받을 수 있도록 async를 사용하며 이 때 내부에서 파일 입출력이 끝나기 까지 수행을 일시정지하기 위해 `deferredInt.await()` 를 사용

   ```kotlin
   async(Dispatchers.Default){
     val value = deferredInt.await()		// 결과값이 오기를 기다림
   }
   ```

4. TextView에 결과값을 세팅하는 작업

   **이 작업은 UI 작업이므로 Dispatchers.Main에서 수행**

   1번 과정에서 세팅한 Dispatchers.Main이 기본 Dispatcher 이므로 Dispatcher 세팅 없이 `launch{}` 만 수행

   Sorting된 값이 올 때까지 기다려야 하므로 `sortedDeferred.await()` 를 수행하며, 해당 값이 왔을 때 SetText

   ```kotlin
   launch{																			//기본 디스패처 = Dispatchers.Main
     val sortedArray = sortedDeferred.await()	// 결과값이 오기까지 대기
     setTextView(sortedArray)									// SetText
   }
   ```




### suspend fun

> 코루틴 블록 내부에서 코루틴 일시 중단
>
> 코루틴 일시 중단은 코루틴 내부에서만 수행
>
> `suspend fun` 은 일시 중단 가능한 함수, 해당 함수 내에 일시 중단이 가능한 작업이 있다는 것을 뜻

코루틴은 기본적으로 일시중단 가능하다.

launch로 실행하든, async로 실행하든 내부에 해당 코루틴을 일시중단 해야하는 동작이 있으면 코루틴은 일시 중단된다.

![image-20220412090425802](https://tva1.sinaimg.cn/large/e6c9d24egy1h16m54fvngj21gw0hm0v1.jpg)

위 그림을 코드로 표현

```kotlin
fun exampleSuspend() {
  // 2. IO Thread에서 작업3 수행
  val job3 = CoroutineScope(Dispatchers.IO).async{
 
    // 5. 작업3 완료
    (1..10000).sortedByDescending{ it }
  }
  
  // 1. Main Thread에서 작업1 수행
  val job1 = CoroutineScope(Dispatchers.Main).launch{
    println(1)
    
    // 3. 작업1의 남은 작업을 위하 작업3의 결과값이 필요하기 때문에 Main Thread는 작업1을 일시중단
    val job3Result = job3.await()
    // 6. 작업3 결과 전달
    
    // 7. 작업1 재개
    job3Result.forEach{
      println(it)
    }
  }
  
  // 4. MainThread에서 작업2 수행 및 완료
  val job2 = CouroutineScope(Dispatchers.Main).launch{
    println("Job2 수행")
  }
}
```

1. Main Thread의 Coroutine1에서 작업1(job1)이 수행되고, Main Thread의 자원을 점유
2. IO Thread의 Coroutine3에서 작업3(job3)이 수행되고, IO Thread의 자원 점유
3. Coroutine1에서 Coroutine3 - 작업3의 결과가 필요, 이 때 Coroutine1은 일시중단
4. Coroutine2가 Main Thread 점유하여 작업2를 수행하고 완료
5. Coroutine3의 작업3 완료
6. Coroutine1은 작업3의 결과를 전달 받음
7. Coroutine1 재개

**결과**

```kotlin
I/System.out : 1 								 //Coroutine1 작업
I/System.out : Job2 수행					//Coroutine2 작업
I/System.out : 10000						 //Coroutine1 작업 재개
	9999
	9998
	9997
	..
```



작업3 (job3)은 IO Thread 위에서 수행되는 async 작업이며 결과값을 반환받는 작업이다.

1 ~ 10000까지 내림차순으로 정렬해서 반환받는 작업이다보니 작업1 (job1)의 println(1) 보다는 많은 시간을 소모할 수 밖에 없다.



이 때문에 job1 코루틴은 job3의 결과를 기다려야만 한다.

따라서 중간에 일시 중단 하는 단계가 필요하다. 비동기 작업을 하다보면 이러한 상황을 맞딱뜨리는 경우가 많은데, 이 때 코루틴에서는 해당 코루틴 작업을 잠시 일시 중단 가능하도록 한다.

이 때문에 일시 중단 가능한 함수는 코투린 내부에서 수행되어야 한다.



**코루틴 일시 중단은 코루틴 블록 내부에서 수행되어야 한다.**

일시 중단을 코루틴 블록 (launch, async) 내부에서 수행하지 않으면 일시 중단 함수로 바꾸라는 오류가 생긴다.

**코루틴이 일시중단이 되려면 수행되는 위치도 코루틴 내부여야 하기 떄문이다.**

이를 해결하는 방법은 두 가지로

1. 일시 중단 작업을 코루틴 내부로 옮기는 것이거나
2. 다른 하나는 `fun` 을 `suspend fun` (일시중단 가능 함수)로 만드는 것이다.



**일시 중단 작업을 코루틴 내부로 옮기기**

```kotlin
fun exampleSuspend(){
  val job3 = CoroutineScope(Dispatchers.IO).async{
    (1..10000).sortedByDescending {it}
  }
  
  CoroutineScope(Dispathcers.Main).launch{
    job3.await()
  }
}
```



**일시 중단 작업을 수행하는 fun을 suspend fun으로 만들기**

suspend fun 을 외부에서 별도 처리 없이 수행하면 오류가 생긴다.

```kotlin
class MainActivity : AppCompatActivity(){
  override fun onCreate(savedInstanceState : Bundle?){
    super.onCreate(savedInstancesState)
    setContentView(R.layout.activity_main)
    
    exampleSuspend()		//에러
  }
}

suspend fun exampleSuspend(){
  val jo3 = CoroutineScope(Dispatchers.IO).async{
    (1..10000).sortedByDescending { it }
  }
  
  job3.await()
}
```

이유는 **suspend fun은 일시중단 가능한 함수를 지칭하는 것이기 때문에, 해당 함수는 무조건 코루틴 내부에서 수행되어야 하기 때문**이다.

```kotlin
class MainActivity : AppCompatActivity(){
  override fun onCreate(savedInstanceState : Bundle?){
    super.onCreate(savedInstancesState)
    setContentView(R.layout.activity_main)
    
    CoroutineScope(Dispatchers.Main).launch{
      exampleSuspend()
    }
  }
}

suspend fun exampleSuspend(){
  val jo3 = CoroutineScope(Dispatchers.IO).async{
    (1..10000).sortedByDescending { it }
  }
  
  job3.await()
}
```

* suspend fun은 suspend fun 내부에서 수행될 수 있다.

```kotlin
suspend fun exampleSuspend2(){
  exampleSuspend1()
}

suspend fun exampleSuspend1(){
  val job3 = CoroutineScope(dispatchers.IO).async{
    (1..10000).sortedByDescending { it }
  }
  job3.await()
}
```





## Coroutine 사용



### 라이브러리 설정

> 버전에 맞게 설정

coroutine-core, coroutine-android 두 개의 라이브러리를 앱 수준의 gradle에 세팅하면 안드로이드에서 코루틴을 사용하기 위한 기본적인 세팅이 완료

```xml
dependencies {
  ...
  implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.5.0'
  implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.5.0'
}
```

coroutines-core 라이브러리는 코루틴을 사용하기 위한 공통적인 라이브러리이다.

Coroutines-android 라이브러리가 필요한 이유는 안드로이드는 스레드를 일반적인 JVM 어플리케이션과 다르게 사용하기 때문이다. 또한 안드로이드 어플리케이션은 Main Thread 에서 충돌이 일어나게 되면 강제로 종료되는 데 이러한 강제 종료가 일어나기 전 예외를 처리하기 위해 별도의 android용 coroutines 라이브러리인 coroutines-android가 필요



### 간단한 예시

```kotlin
Log.d(TAG, "doing something in mainThread")		//1

GlobalScope.launch{														//2 
  delay(3000)
  Log.d(TAG, "done something in Coroutine")		//3
}

Log.d(TAG, "done in mainThread")							//4
```

1. main thread에서 로그 출력
2. launch{...} 코드 블록은 코루틴에서 작업을 수행하는 명령어이다. 괄호 안의 코드들이 비동기적으로 수행
3. 코루틴에서 3초 쉬고, 로그를 출력
4. 코루틴을 실행하고 메인 쓰레드에서 다시 로그 출력



**실행결과**

```kotlin
10-26 19:52:42.975  7699  7699 D MainActivity: doing something in main thread
10-26 19:52:42.985  7699  7699 D MainActivity: done in main thread
10-26 19:52:45.992  7699  7727 D MainActivity: done something in Coroutine
```

메인 쓰레드의 로그가 모두 출력되고, 코루틴은 delay로 인해 나중에 로그가 출력

코루틴은 비동기적으로 수행되기 때문에 메인쓰레드 코드들이 먼저 호출되었다.



### launch, async

> 코루틴을 실행하는 명령어

`launch`, `async` 는  코루틴을 실행하는 명령어라는 공통점이 있지만 아래와 같은 차이점이 있다.

- launch 는 리턴 값이 없다.
- async 는 Deferred<T> 객체를 리턴한다.



Deferred<T> 클래스는 `await()` 메소드를 제공한다.

이 메소드는 작업이 완료되는 것을 기다리고 T 타입의 객체를 리턴한다.

```kotlin
//Deferred.kt
public interface Deferred<out T> : Job {
  public suspend fun await() : T
  ...
}
```



#### launch, async 사용 방법

```kotlin
GlobalScope.launch{																//1
  launch {																				//2
    Log.d(TAG, "Launch has No return value")
  }
  
  val value = async {															//3
    1 + 2																					//4
  }.await()																				//5
  
  Log.d(TAG, "Async has return value : $value")
}
```

1. GlobalScope.launch 는 코루틴을 실행
2. GlobalScope.launch{...} 안에 launch{...}로 다른 코루틴을 실행
3. async{...} 로 코루틴 실행
4. "1 + 2" 연산 후 3을 리턴
5. await() 는 async의 코루틴이 끝날 때까지 기다리고 결과를 리턴한다.



**실행결과**

```kotlin
10-26 20:52:13.597  8158  8187 D MainActivity: Launch has NO return value
10-26 20:52:13.599  8158  8186 D MainActivity: Async has return value: 3
```



### Suspend functions

코루틴 안에서 일반적인 메소드는 호출 할 수 없다.

코루틴의 코드는 잠시 실행을 멈추거나 (suspend) 다시 실행 될(resume) 수 있기 때문이다.

코루틴에서 실행할 수 있는 메소드를 만드려면 함수를 정의 할 때 `suspend` 를 붙여 주면 된다.

suspend 함수는 안에서 다른 코루틴을 실행 할 수도 있다.

```kotlin
GlobalScope.launch{
  doSomething()
  Log.d(TAG, "done something")
}

suspend fun doSomething(){
  GlobalScope.launch{
    sleep(1000)
    Log.d(TAG, "do something in a suspend method")
  }
}
```



**실행 결과**

```kotlin
10-26 21:05:18.990  8418  8446 D MainActivity: done something
10-26 21:05:19.990  8418  8447 D MainActivity: do something in a suspend method
```



### Thread

코루틴은 여러 함수를 번갈아가면서 동작된다.

코루틴이 실행되는 쓰레드를 지정할 수 있다.

쓰레드를 지정해줘야 하는 이유는, 작업이 종류에 따라서 빠르게 처리되어야 하는 것이 있고, 안드로이드의 UI 작업은 Main thread에서만 수행되어야 하기 때문에 이런 경우 Main thread에서 코루틴이 실행되도록 해야 한다.



`launch()` 에 인자로 쓰레드 타입을 넘겨주면, 코루틴은 그 쓰레드에서 실행된다.

```kotlin
GlobalScope.launch(Dispatchers.IO){
  doOnIOthread()
}
```

안드로이드는 3개의 Dispatchers를 제공한다.

- Dispatchers.Main : 안드로이드의 메인 쓰레드, UI 작업은 여기서 처리
- Dispatchers.IO : Disk 또는 네트워크에서 데이터 읽는 I/O 작업 처리, ex) 파일 읽기, AAC의 Room 등
- Dispatchers.Default : 그 외 CPU에서 처리하는 대부분의 작업들은 이 쓰레드에서 처리



예시

```kotlin
GlobalScope.launch(Dispathcers.Main){						// 1
  val userOne = async(Dispathcers.IO){					// 2
    fetchFirstUser()
  }
  val userTwo = async(Dispathcers.Default){			// 3
    fetchSecondUser()
  }
  showUsers(userOne.await(), userTwo.await())		// 4
}
```

1. 코루틴을 Main 쓰레드에서 실행
2. 이 작업은 IO 쓰레드에서 수행
3. 이 작업은 Default 쓰레드에서 수행
4. 이 코드는 Main 쓰레드에서 실행되며, 2와 3 작업이 모두 끝날 때 까지 기다리고 결과를 출력



`withContext()` 라는 메소드도 있다.

이것은 `async` 와 동일한 역할을 하는 키워드이다.

차이점은 `await()` 를 호출할 필요가 없다는 것이다.

결과가 리턴될 때까지 기다린다.

```kotlin
suspend fun <T> withContext(
	context : CoroutineContext,
  block : suspend CoroutineScope.() -> T
): T (source)
```



예시

```kotlin
GlobalScope.launch(Dispathcers.IO){						// 1
  Log.d(TAG, "Do something on IO thread")			
  val name = withContext(Dispathcers.Main){		// 2
    sleep(2000)
    "My name is Sleam Shady"
  }
  																						// 3
  Log.d(TAG, "Result : $name")								// 4
}
```

1. 이 코드는 IO 쓰레드에서 실행
2. 이 코드는 Main 쓰레드에서 실행
3. withContext() 다음 코드를 수행하지 않는다. await() 를 호출한 것 처럼 결과가 리턴되기를 기다린다.
4. withContext() 의 코루틴이 모두 수행되면 이 코드가 수행된다.



실행결과

```kotlin
10-26 22:44:16.488  9723  9752 D MainActivity: Do something on IO thread
10-26 22:44:18.649  9723  9752 D MainActivity: Result : My name is Sleam Shady
```



### Scope

Scope는 코루틴이 실행되는 범위이다.

위의 예시는 모두 GlobalScope를 사용했는데, 이 스코프는 Application이 종료될 때 까지 코루틴을 실행시킬 수 있다.

만약 Acitivity에서 코루틴을 GlobalScope 영역에서 실행시켰다면, Acitivity가 종료되도 코루틴은 작업이 끝날 때까지 동작한다.

Acitivity 에서보여줄 이미지를 다운받고 있는데 Activity가 종료되었다면 그 작업은 불필요한 리소스를 낭비하고 있는 것이다.

Acitivity가 종료될 때 실행 중인 코루틴도 함께 종료되길 원하다면 Acitivity의 Lifecycle과 일치하는 Scope에 코루틴을 실행시키면 된다.



Activity scope는 없기 때문에 Activitiy의 Lifecycle과 일치하는 Scope를 만들어줘야 한다.

```kotlin
class MainActivity : AppCompatActivity(), CoroutineScope {		// 1
    private lateinit var job: Job     												// 2
    override val coroutineContext: CoroutineContext   				// 3
        get() = Dispatchers.Main + job
}
```

1. CoroutineScope 인터페이스 구현

2. Job 객체 선언

3. CoroutineScope 인터페이스의 CoroutineContext 변수를 오버라이드

   Dispatcher.Main 에 위에서 생성한 Job을 더한다.



```kotlin
override fun onCreate(savedInstanceState : Bundle?){
  super.onCreate(savedInstanceState)
  job = Job()																					// 1
  ....
}

override fun onDestroy(){
  super.onDestroy()
  job.cancel()																				// 2
}
```

1. Job 객체 생성
2. Activity가 종료될 때 수행 중인 Job이 있다면 취소



이 Activitiy는 CoroutineScope를 구현했기 때문에 코루틴을 실행할 때 다음처럼 `launch{}` 키워드만 입력하면 된다.

```kotlin
launch {
  Log.d(TAG, "do something")
  ...
}
```

위처럼 클래스 내에 객체를 생성해서 Scope를 만들 수 있다.



다음은 ViewModel과 동일한 Lifecycle을 갖는 Scope를 생성하는 예제

ViewModel 객체는 Activity가 완전히 소멸되면 함께 소멸된다.

```kotlin
class MyViewModel : ViewModel() {
    private val job = Job()     																		// 1
    private val uiScope = CoroutineScope(Dispatchers.Main + job)    // 2

    fun doSomeOperation() {
        uiScope.launch {    																				// 3
            val deferred = async(Dispatchers.Default) {
                10 + 10
            }
            Log.d(TAG, "await: ${deferred.await()}")
        }
    }

    override fun onCleared() {    																	// 4
        super.onCleared()
        job.cancel()     																						// 5
    }
}
```

1. Job 객체 생성
2. CoroutineScope 객체 생성
3. 코루틴 실행 시 위에서 생성한 uiScope 사용하여 uiScope.launch{...}
4. ViewModel 객체가 소멸될 때 onCleared() 메소드가 호출
5. 실행 중인 코루틴이 있다면 Job을 취소



### Exception handling

코루틴 안에서 Exception 이 발생하면 앱이 갑자기 죽는다.

예외 처리는 다음과 같이 할 수 있다.

```kotlin
GlobalScope.launch(Dispatchers.IO + handler){														// 1
  launch {
    throw Exception()																										// 2
  }
}

val handler = CorutineExceptionHandler { coroutineScope, exception ->   // 3
    Log.d(TAG, "$exception handled!")
}
```

1. launch()의 인자에 플러스 연산자로 "handler"를 추가한다. 예외가 발생하면 이 handler로 콜백이 전달되어 예외처리
2. 예외가 발생
3. 예외 처리



**결과**

```kotlin
10-26 22:54:16.756 10279 10306 D MainActivity: java.lang.Exception handled!
```



`async` 나 `withContext` 를 사용하는 경우 위의 방법으로 예외처리가 안된다.

아래와 같이 try-catch 구문으로 예외처리를 해야 한다.

```kotlin
GlobalScope.launch(Dispatchers.IO){
  try {
    val name = withContext(Dispatchers.Main){
      throw Exception()
    }
  } catch (e : Exception){
    Log.d(TAG, "$e handled!")
  }
}
```







---

참고사이트

쾌락코딩 깃헙 : https://wooooooak.github.io/kotlin/2019/08/25/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%BD%94%EB%A3%A8%ED%8B%B4-%EA%B0%9C%EB%85%90-%EC%9D%B5%ED%9E%88%EA%B8%B0/

Gisdeveloper 코루틴 예제 : http://www.gisdeveloper.co.kr/?p=10279

티스토리 : https://whyprogrammer.tistory.com/596 



Coroutines 101 한글번역 : https://seunghyun.in/android/7/#synchronous

Coroutines 공식 가이드 한글번역 : https://myungpyo.medium.com/reading-coroutine-official-guide-thoroughly-part-1-98f6e792bd5b

KotlinWorld : https://kotlinworld.com/155?category=973476



사용

코드차차 : https://codechacha.com/ko/android-coroutine/

