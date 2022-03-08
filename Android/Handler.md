# Handler

> worker thread 에서 main thread로 메시지를 전달해주는 역할을 하는 클래스



사전적의미 : 처리하는 사람, 조언자, 조련사

무언가를 만지로, 다루는 뉘앙스의 느낌



> 스레드란, 프로세스 내에서 "순차적으로 실행되는 실행 흐름"의 최소 단위이며,
>
> 프로그램의 main() 함수로부터 시작되는 최초 실행 흐름 또한 하나의 스레드이며, 이를 메인 스레드 라고 한다.
>
> 안드로이드 앱에서 메인 스레드는 메시지 큐 (Message Queue) 수신을 대기하는 루프를 실행하며, 사용자 입력과 시스템 이벤트, 화면 그리기 등의 메시지가 수신되면 각 메시지에 매핑된 핸들러의 메서드를 실행 한다.



어떠한 경우에 스레드를 사용해야 할까?

구현하고자 하는 기능이 메인 스레드와 병행적으로 실행되어야 하는 가를 확인하는 것이다.

다르게 말하면, 어떤 기능을 메인 스레드에 구현했을 때, 메인 스레드 동작에 영향을 주는가를 확인하는 것이다.

실행 시간이 오래 걸리거나, 외부 데이터를 수신하기 위해 대기 상태에 머물러야 하는 경우, 이는 메인 스레드의 동작에 영향을 줄 수 있기 때문에 별도의 스레드로 작성해야 한다.



메인 스레드와 병행적으로 실행되어야 하는 기능을 구현하기 위해 스레드를 사용하는 것은, 두 개 이상의 스레드로 프로그래밍 하는 것, 멀티 스레드 (Multi Thread) 프로그래밍에서는 고려해야 할 사항과 풀어야 할 이슈들이 싱글 스레드 (Single Thread) 프로그래밍에 비해 훨씬 많아지기 때문에 복잡해진다.



멀티 스레드 프로그래밍에서 가장 쉽게 만날 수 있는 이슈인 "스레드 간 통신"을 알아보자



## 스레드 통신 (Thread Communication)

> 하나의 스레드에서 다른 스레드로 데이터를 전달하는 것을 스레드 통신 (Thread Communication) 이라고 한다.



프로그램에서 실행되는 모슨 스레드는 기본적으로 독립적인 실행 흐름을 가진다. 이 말은, 스레드가 실행되고 나면 다른 스레드로부터 간섭도 받지 않고, 다른 스레드가 어떻게 실행되는지 알 수 없다

이러한 독립성은 스레드의 본질적 특성을 얘기하는 것일 뿐, 개발자가 스레드의 실행 상태 또는 결과를 다른 스레드로 전달하도록 의도적으로 구현해야 하는 경우가 많다.



ex) USB 메모리에 있는 파일을 내부 스토리지로 복사해야하는 상황

파일 복사 스레드의 진행 상태를 메인 스레드로 메시지 형태로 전달하고, 메인 스레드에서는 그 값을 사용하여 현재 진행 상태를 화면에 표시한다. 

![스레드 통신 예제 4](https://t1.daumcdn.net/cfile/tistory/99DE91445CEB795E36)

이렇게, 하나의 스레드에서 다른 스레드로 데이터를 전달하는 것을 스레드 통신 이라고 한다.



## 안드로이드 스레드 통신, 핸들러 (Handler)



안드로이드에서 사용 할 수 있는 스레드 통신 방법은 여러가지 있지만, 일반적으로 사용할 수 있는 방법은 핸들러 (Handler)를 통해 메시지 (Message)를 전달하는 방법이다.

![스레드 통신 흐름](https://t1.daumcdn.net/cfile/tistory/994915405CEB795E27)



**핸들러의 각 요소**

### 메시지 (Message)

> 핸들러를 사용하여 데이터를 보내기 위해, 전달할 데이터를 저장하는 클래스

`android.os.Message`

스레드 통신에서 핸들러를 사용하여 데이터를 보내기 위해서, 데이터 종류를 식별할 수 있는 식별자와 실질적인 데이터를 저장한 객체, 그리고 추가 정보를 전달할 객체가 필요하다.

즉, 전달할 데이터를 한 곳에 저장하는 역할을 하는 클래스 이다.



하나의 데이터를 보내기 위해서는 한 개의 `Message` 인스턴스가 필요하며, 일단 데이터를 담은 `Message` 객체를 핸들러로 보내면 해당 객체는 핸들러와 연결된 메시지 큐 (Message Queue)에 쌓인다.



### 메시지 큐 (MessageQueue)

> `Message` 객체를 들어오는 순서에 따라 저장하고, 먼저 들어온 객체를 순서대로 처리하는 클래스
>
> `Message` 객체 리스트를 관리하는 클래스

`android.os.MessageQueue`

`Message` 객체를 큐(Queue) 형태로 관리하는 자료 구조이다. 

First In First Out (FIFO) 방식으로 동작하여, 메시지는 큐에 들어온 순서에 따라 차례대로 저장되고, 먼저 들어온 `Message` 객체는 순서대로 처리 된다.



안드로이드 메시지 큐는 앱의 메인 스레드에서 기본적으로 사용되고 있다.

개발자가 `MessageQueue` 객체를 직접 참조하여 메시지에 전달하거나, 메시지를 가져와서 처리 하지 않는다.

메시지 전달은 메시지 큐에 연결된 핸들러 (Handler)를 통해서,

메시지 큐로부터 메시지를 꺼내고 처리하는 역할은 루퍼 (Looper)가 수행하기 때문이다.



### 루퍼 (Looper)

> 메시지 큐로 부터 메시지를 꺼내온 다음, 해당 메시지와 연결된 핸들러를 호출하는 역할

`android.os.Looper`

`MessageQueue` 는 `Message` 객체 리스트를 관리하는 클래스 일뿐, 메시지 처리를 위한 핸들러를 실행시키진 않는다.

메시지 루프, 즉 메시지 큐로 부터 메시지를 꺼내온 다음, 해당 메시지와 연결된 핸들러를 호출하는 역할은 루퍼가 담당한다.



안드로이드 앱 메인 스레드에는 `Looper` 객체를 사용하여 메시지 루프를 실행하는 코드가 이미 구현되어 있고, 해당 루프 안에서 메시지 큐의 메시지를 꺼내어 처리하도록 만들어져 있다.

메인 스레드에서 메시지 루프와 관련된 코드를 개발자가 추가 작성할 필요가 없다.

개발자 할 일은 메인 스레드로 전달할 `Message` 객체를 구성하고, 스레드의 메시지 큐에 연결된 핸들러(Handler)를 통해 해당 메시지를 보내기만 하면 된다.



### 핸들러 (Handler)

> 스레드의 루퍼(Looper)와 연결된 메시지 큐로 메시지를 보내고 처리
>
> 메인 스레드의 메시지 처리 흐름에서, 메시지 전달과 처리를 위해 개발자가 접근 할 수 있는 창구 역할

`android.os.Handler`

스레드와 연결된 핸들러를 얻기 위해서는 `Handler` 클래스 인스턴스를 생성하기만 하면 된다.

그러면 새로운 `Handler` 인스턴스는 자동으로 해당 스레드와 메시지 큐에 연결되고, 그 시점 부터 핸들러를 통해 메시지를 보내고 처리 할 수 있다.



## 핸들러 사용 방법



스레드 통신 수행 절차

1. 수신 스레드에 핸들러 객체 생성
2. `handlerMessage()` 오버라이드
3. 핸들러 객체를 통해 `Message` 객체 할당
4. `Handler.sendMessage()` 메서드로 전달



### 메시지 수신 스레드

> Handler 객체생성, `handlerMessage()` 메서드 오버라이드

핸들러는 생성과 동시에, 코드가 실행된 스레드에 연결(bind) 된다. 좀 더 정확히는, `Handler` 클래스 생성자에서 현재 스레드의 루퍼(Looper), 메시지 큐 (MessageQueue)에 대한 참조를 가지게 된다.

이후 단계에서 메시지를 보낼 떄 이 참조를 사용하여 메시지 큐에 메시지를 넣는다.

```kotlin
class HandlerTest : AppCompatActivity(){

  val MSG_A = 0
  val MSG_B = 1
  
  //핸들러 객체 생성, hnadlerMessage() 메서드 오버라이드
  val mHandler: Handler = object: Handler(){
    orerride fun handleMessage(msg: Message){
      when(msg.what){
        MSG_A -> {}
        MSG_B -> {}
      }
    }
  }
 }
```

핸들러 생성 후 , 핸들러에서 수신한 메시지를 처리하기 위해 `handleMessage` 메서드를 오버라이드 하는 것이다.



`handleMessage()` 메서드는 메시지 루프를 통해 메시지 큐(MessageQueue)로부터 꺼낸 메시지를 처리할 수 있도록, 루퍼에 (Looper)의해 실행되는 메서드이다.

다른 스레드로부터 전달된 데이터는 `msg` 인스턴스에 담겨져 있으며, 일반적으로, 정수 타입인 `what` 변수의 값에 따라 조건문을 사용하여 처리 한다.



### 메시지 송신 스레드

> 수신 스레드의 핸들러 객체 참조를 통해 메시지 객체 획득

메시지를 송신하는 곳에서는 먼저, 앞서 생성한 수신 스레드의 핸들러 객체 참조를 획득해야 한다.

메인 스레드인 경우, 액티비티의 클래스 변수로 핸들러 객체를 선언하고, 액티비티 참조를 통해 핸들러 객체를 참조할 수 있다.

액티비티 내에서 스레드를 생성했다면, 핸들러 객체를 바로 참조 할 수 있다.

```kotlin
class HandlerTest : AppCompatActivity(){

  val MSG_A = 0
  val MSG_B = 1
  
  //핸들러 객체 생성, hnadlerMessage() 메서드 오버라이드
  val mHandler: Handler = object: Handler(){
    orerride fun handleMessage(msg: Message){
      when(msg.what){
        MSG_A -> {}
        MSG_B -> {}
      }
    }
  }
  //수신 스레드 핸들러 객체 참조를 통해 메시지 객체 획득
  inner class NewThread : Thread(){
     override fun run() {
       while(true){
         val message = mHandler.obtainMessage()
         message.what = MSG_A
         message.arg1 = 2
         message.arg2 = 3
         message.obj = "아무타입"
         
	//메시지 보내기
         mHandler.sendMessage(message)
			}
		}
	}
}
```

메시지 객체를 획득하기 위해서는 `Handler` 의 `obtainMessage()` 메서드를 사용한다.

이 메서드는 글로벌 메시지 풀 (Global Message Pool)로 부터 메시지를 가져오는 데, 정적으로 생성된 객체이다.



`Message` 클래스에는 아래와 같은 변수들이 있다.

| 클래스 변수           | 설명                                             |
| --------------------- | ------------------------------------------------ |
| int **what**          | 메시지 종류 식별을 위한 사용자 정의 메시지 코드. |
| int **arg1**          | 메시지를 통해 전달되는 정수 값 저장.             |
| int **arg2**          | 메시지를 통해 전달되는 정수 값 저장.             |
| Object **obj**        | 수신 스레드에 전달할 임의의 객체 저장.           |
| Messenger **replyTo** | 해당 메시지에 대한 회신 용 메신저.               |
| int **sendingUid**    | 메시지를 보낸 uid를 가리키는 필드.               |



`Message` 객체를 획들할 때 모든 클래스 변수를 사용해야 하는 것은 아니기 떄문에, `obtainMessage()` 메서드는 사용할 필드에 따라 여러 형태의 메서드가 존재한다.

| 메서드 프로토타입                                            | 설명                                                       |
| ------------------------------------------------------------ | ---------------------------------------------------------- |
| Message **obtainMessage**()                                  | 메시지의 target이 핸들러 자신으로 지정된 Message 객체 리턴 |
| Message **obtainMessage**(int what)                          | what이 지정된 Message 객체 리턴.                           |
| Message **obtainMessage**(int what, int arg1, int arg2)      | what, arg1, arg2가 지정된 Message 객체 리턴.               |
| Message **obtainMessage**(int what, Object obj)              | what, obj가 지정된 Message 객체 리턴.                      |
| Message **obtainMessage**(int what, int arg1, int arg2, Object obj) | what, arg1, arg2, obj가 지정된 Message 객체 리턴.          |



### 메시지 보내기

> `Handler.sendMessage()` 메서드를 사용하여 메시지 객체를 수신 스레드에 보내는 것



`obtainMessage()` 메서드로 획득한 메시지 객체에 보내고자 하는 데이터를 채우고 나면, 마지막으로 할 일은 `Handler.sendMessage()` 메서드를 사용하여 메시지 객체를 수신 스레드에 보내는 것이다.



위 코드에서 `mHandler.sendMessage(message)`



`sendMessage()` 메서드 도 파라미터, 메시지가 보내지는 시점, 메시지 큐 내 위치 등에 따라 다양한 프로토 타입이 존재한다.

| 메서드 프로토타입                                            | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| boolean **sendEmptyMessage**(int what)                       | Message 클래스 변수 중 what 멤버만 채워진 Message 객체 전달. |
| boolean **sendEmptyMessageAtTime**(int what, long uptimeMillis) | uptimeMillis에 지정된 시각에, what 멤버만 채워진 Message 객체 전달. |
| boolean **sendEmptyMessageDelayed**(int what, long delayMillis) | 현재 시각에서 delayMillis 만큼의 시간 후에, what 멤버만 채워진 Message 객체 전달. |
| boolean **sendMessage**(Message msg)                         | Message 객체 전달. 메시지 큐의 가장 마지막에 msg 추가.       |
| boolean **sendMessageAtFrontOfQueue**(Message msg)           | Message 객체 전달. 메시지 큐의 가장 처음 위치에 msg 추가.    |
| boolean **sendMessageAtTime**(Message msg, long uptimeMillis) | uptimeMillis에 지정된 시각에, Message 객체 전달.             |
| boolean **sendMessageDelayed**(Message msg, long delayMillis) | 현재 시각에서 delayMillis 만큼의 시간 후에, Message 객체 전달. |



`sendMessage()` 외에 `Runnable` 객체를 보낼 때 사용할 수 있는 `post()`메서드가 존재한다.



## 핸들러 관련 참고 사항

### 스레드 통신의 대상은 자기 자신도 포함

스레드 통신을 위해 핸들러를 사용할 때, 다른 스레드 뿐만 아니라 스레드 통신의 대상이 자기 자신이 될 수도 있다.

![스레드 통신의 대상이 자기 자신](https://t1.daumcdn.net/cfile/tistory/9999B93C5CEB795E0D)



### 핸들러는 스레드 당 반드시 1개만 생성해야 하는가?

핸드러는 여러 개 만들 수 있다.

하나의 핸들러에서 모든 메시지를 처리하는 것 보다, 메시지 종류 및 기능에 따라 여러 개의 핸들러로 나우어서 처리하는 게 더 좋다.

물론, 메시지를 보내는 스레드에서 적절한 핸들러를 선택하여 메시지를 보내야 한다.

```kotlin
val mNetworkHandler: Handler = object : Handler() {
  override fun handlerMessage(msg: Message){
    // network code
  }
}

val mDeviceHandler: Handler = object : Handler(){
  override fun handlerMessage(msg: Message){
    // device code
  }
}
```



### Handler() 가 Deprecated된 이유

원래는 Handler를 선언할 때 생성자에 아무것도 넣어주지 않아도 되었다.

하지만 

![img](https://media.vlpt.us/images/kimbsu00/post/49048874-dc84-40dc-a28b-53b51da72ac1/2.PNG)

`Handler` 가 생성되는 동안, `Looper` 가 암묵적으로 선택되면 여러가지 버그가 발생할 수 있고, 이러한 가능성을 차단하기 위해서는 명시적으로 `Looper` 를 선언해야 하므로, `Handler()` 와 `Handler(Handler.Callback) 이 deprecated 되었다.



해결방법은 생성자로 `Looper` 를 넣어서 사용하면 된다.

`Handler(Looper.getMainLooper())`

```kotlin
val mHandler : Handler = object : Handler(Looper.getMainLooper()){
  override fun handleMessage(msg : Message){
    // code
  }
}
```





---

참고 사이트

개발자 레시피 티스토리 블로그 : https://recipes4dev.tistory.com/166?category=768056

코드차차 : https://codechacha.com/ko/android-handler-basic/

재영 티스토리 블로그 : https://jae-young.tistory.com/25

Kimbsu velog : https://velog.io/@kimbsu00/Android-4
