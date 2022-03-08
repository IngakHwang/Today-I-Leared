# Android Thread 연습

> 메인 스레드와 병행적으로 실행되어야 할 작업은 스레드로 작성한다.
>
> 메인 스레드에서는 실행 시간 또는 대기 시간이 긴 작업의 실행을 피해야 한다.
>
> UI를 변경하는 작업은 반드시 메인 스레드에서 실행되어야 한다.
>
> 핸들러(Handler)를 사용하여 메인 스레드로 메시지를 보낼 수 있다.



## 스레드 없이 변화하는 화면 구현

> 안드로이드 앱의 메인 스레드에서는 무한 루프, 실행 시간이 긴 작업, `Thread.sleep()`을 통한 과도한 대기 코드를 피해야 한다.

```kotlin
class MainActivity : AppCompatActivitiy(){
  var clockTextView: TextView? = null
  
  override fun onCreate(savedInstanceState: Bundle?){
    
    clockTextView = findViewById(R.id.clock)
    
    val cal = Calendar.getInstance()
    val sdf = SimpleDateFormat("HH:mm:ss")
    
    while(true){
      val strTime = sdf.format(cal.getTime())
      clockTextView?.text = strTime
      
      Thread.sleep(1000)
    }
  }
}
```

1초마다 현재 시각을 얻어와서 화면에 표시하는 코드



앱 빌드 시 

![스레드 없이 무작정 만들어보기 예제 실행 화면](https://t1.daumcdn.net/cfile/tistory/9985383D5C541EAE06)

텍스트뷰 조차 화면에 표시되지 않는다.



**안드로이드 앱의 초기화 코드를 작성하는 메서드인 onCreate() 메서드는 어디서 호출, 실행되는 것일까?**

`onCreate()` 가 어떠한 이벤트 메시지가 수신되었을 때 실행되는 메서드이고, 안드로이드 앱의 메인 스레드에서 실행된다는 것을 어렴풋이 기억하자



그런데 위에서 작성한 코드를 보면, `onCreate()` 메서드에서 무한 루프를 실행해 버려, 메인 스레드 입장에서 `onCreate()` 메서드에서 초기화 작업을 마치고나서, 다시 메시지 큐를 확인하여 다른 이벤트(화면 그리기, 터치 입력 처리등)들을 처리 해야 하는데, `onCreate()` 메서드가 끝나지 않는 것이다.



이러한 이유로, 안드로이드 앱의 메인 스레드에서는 무한 루프나 실행 시간이 긴 작업, 또는 `Thread.sleep()`을 통ㅏㄴ 과도한 대기 등의 코드 작성을 피해야 한다.



## 스레드 사용

> UI 그리기 기능은 반스디 메인 UI 스레드에서 실행되어야 한다.



위 코드의 문제는, 지속적으로 실행되어야 하는 작업을 메인 스레드(`onCreate()` )에서 실행했기 때문에, 메인 스레드의 다른 코드가 더 이상 실행되지 않는 것이다. 사용자 입력 처리도, 화면 갱신도 못하는 것이다.



그럼 메인 스레드와 분리되어 동시적으로 실행되어야 하는 작업을 별도의 스레드로 작성

![앱 구조 설계(스레드 적용)](https://t1.daumcdn.net/cfile/tistory/9939D83A5C541EAE04)



```kotlin
class MainActivity : AppCompatActivitiy(){
  var clockTextView: TextView? = null
  
  override fun onCreate(savedInstanceState: Bundle?){
    
    clockTextView = findViewById(R.id.clock)
    
    thread(start = true){
      var cal = Calendar.getInstance()
      var sdf = SimpleDateFormat("HH:mm:ss")
      
      while(true){
        val strTime = sdf.format(cal.time)
        clockTextView?.text = strTime
        
        Thread.sleep(10000)
      }
    }
  }
}
```



코드 자체는 `onCreate()` 메서드 내부에 작성되어 있지만, 메인 스레드와 분리된 새로운 스레드에서 실행된다. 그래서 `while loop` 로 인하여 메인 스레드의 루프가 실행되지 않는 문제가 해결된다.

그러면 메인 스레드가 정상적으로 다른 이벤트를 처리 할 수 있을까?



앱 빌드 시

​	: 현재 시각 표시 되더니 앱이 종료된다. 원인은 출록된 로그 메시지를 확인

![Only the original thread that created a view hierarchy can touch its views](https://t1.daumcdn.net/cfile/tistory/99FAC2445C541EAE37)

에러 메시지

`"Only the original thread that created a view hierarchy can touch its views."`

​	: 뷰 계층을 생성한 원래 스레드만이 해당 뷰를 건드릴 수 있다.



코드에서 화면에 텍스트뷰에 현재 시각을 표시하기 위해, 새로 만든 스레드 내에서 텍스트뷰의 text를 수정하였다.

`clockTextView?.text = 변경값` 은 UI 그리기 기능이 실행되는 것인데, 메인 스레드가 아닌 다른 스레드에서 화면을 그리는 기능이 실행되었기 때문에 에러가 발생한다.



## 스레드 간 통신



위의 코드들은 안드로이드 메인 스레드가 아닌 스레드에서 뷰(View)에 대한 직접적인 접근의 문제를 알 수 있었다.

위 문제를 해결하기 위해선, "스레드 간 통신"을 적용 해야 한다.



스레드 간 통신 : 하나의 스레드에서 다른 스레드로 메시지를 보내는 것



새로 만든 스레드(1초 마다 메시지 전달)에서 메인 스레드(현재 시각 화면에 표시)로 메시지를 보내면 된다.

![앱 구조 설계(스레드 간 통신을 적용)](https://t1.daumcdn.net/cfile/tistory/99E4C24D5C541EAE0E)



```kotlin
class MainActivity : AppCompatActivitiy(){
  var clockTextView: TextView? = null
  
  override fun onCreate(savedInstanceState: Bundle?){
    
    clockTextView = findViewById(R.id.clock)
    
		@SuppressLint("HandlerLeak")
    mHandler = object : Handler() {
      override fun handleMessage(msg: Message){
        val cal = Calendar.getInstance()
        
        val sdf = SimpleDateFormat("HH:mm:ss")
        val strTime = sdf.format(cal.time)
        
        clockTextView?.text = strTime
      }
    }
    
    thread(start = true){
      while(true){
        Thread.sleep(1000)
        mHandler?.sendEmptyMessage(0)
      }
    }
  }
}
```

위 코드에서 핵심은 핸들러를 만들고(`mHandler = object : Handler()~~`) 수신 메시지를 처리 하는 코드 (`handleMessage`) , 메인 스레드의 핸들러를 통해 메시지를 전달 하는 코드(`sendEmptyMessage`) 이다.



위의 내용을 정리 하면

메인 스레드와 병행적으로 실행되어야 할 작업은 스레드로 작성한다.

메인 스레드에서는 실행 시간 또는 대기 시간이 긴 작업의 실행을 피해야 한다.

UI를 변경하는 작업은 반드시 메인 스레드에서 실행되어야 한다.

핸들러(Handler)를 사용하여 메인 스레드로 메시지를 보낼 수 있다.



---

참고 사이트

개발자 레시피 티스토리 블로그 : https://recipes4dev.tistory.com/150?category=768056