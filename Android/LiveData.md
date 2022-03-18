# LiveData

> 데이터가 변경되면 알림을 받을 수 있는 DataHolder 클래스
>
> DataHoler 클래스는 어떤 동작은 거의 하지 않고 데이터만 가지는 클래스



알림을 받는 것 = 관찰한다



관찰 가능한 객체나 데이터를 나타내는 클래스는 Observerable 클래스이다.

그리고 Observerable 객체에 변경이 발생하는 것을 알려주고 싶을 때 사용하는 것이 Observer 인터페이스이다.



LiveData는 기본적으로 Observerable 클래스와 같이 동작한다.

중요한 차이점은 생명주기를 인식한다는 것이다.



어떻게 인식을 할까?

Activity, Fragment 에는 LifecycleOwner 인터페이스가 기본적으로 구현되어 있다.

LifecycleOwner는 getLifecycle() 메서드를 통해 Lifecycle이라는 객체를 가지고 온다.

LiveData는 LifecycleOwner가 구현된 객체(Activity, Fragment) 와 연결되는 Observer를 등록할 수 있고, 그 객체에 생명주기를 Observer가 인식 할 수 있다.



Observer가 인식한 생명주기가 STARTED, RESUMED 상태이면 활성 상태라고 한다.

LiveData는 활성 상태인 Observer에게만 변경사항을 알린다.

![image-20220318163223447](https://tva1.sinaimg.cn/large/e6c9d24egy1h0e2ljyn6hj20i60bn3ys.jpg)



## LiveData 장점

1. 편리한 UI, 데이터 상태 동기화
   - 데이터가 변경될 때마다 Observer를 통해서 콜백을 받기 때문에 Observer에서 변경된 데이터로 UI를 업데이트하도록 구현하면 별도로 UI 업데이트에 대한 신경을 쓰지 않아도 된다.
2. 메모리 누수 없음
   - Observer가 LifecycleOwner가 구현된 객체와 연결되어 있기 때문에 해당 객체의 생명주기가 destory 되면 Observer도 메모리 해제 된다.
3. 비활성 Activity에 의한 충돌 없음
   - Observer의 생명주기가 비활성 상태일 때 LiveData의 어떤 이벤트도 수신하지 않는다.
4. 별도의 생명주기 관리 불필요
   - LiveData는 Observer를 통해서 생명주기의 상태 변경을 알고 있기에 생명주기에 대해서 자동 관리 된다.
5. 항상 최신 데이터 유지
   - 생명주기가 비활성에서 활성으로 바뀌는 경우에 최신 데이터를 수신한다.
     - ex) Activity 백그라운드에 있다가 포그라운드로 오는 경우 또는 화면 회전으로 Activity가 다시 생성되는 경우에 최신 데이터를 받는다.
6. 리소스 공유
   - LiveData가 생명주기를 인식하기 때문에 여러 Activity, Fragment, Service에 LiveData를 공유할 수 있다.
     - LiveData를 상속하여 커스텀 LiveData를 만들어 사용할 Activity, Fragment, Service 연결



## LiveData 이해를 위한 간단한 예제

버튼을 눌렀을 때 

LiveData의 값을 변경하고

Observer가 알림을 받고

변경된 값을 버튼의 텍스트에 적용



```kotlin
class MainActivity : AppCompatActivity(){
  var liveData : MutableLiveData<String> = MutableLiveData()
  
  override fun onCreate(savedInstanceState: Bundle?){
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    
    val button = findViewById<TextView>(R.id.btn_change)
    
    val observer = Observer<String>{
      button.text = it
    }
    
    liveData.observe(this, observer)
    
    button.setOnclickListener{
      liveData.value = if(button.text.equals("ON")) "OFF"
      	else "ON"
    }
  }
}
```



```xml
~~

<Button
        android:id="@+id/btn_change"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="ON"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

~~
```



버튼 클릭 했을 때, LiveData 값 변경이 발생한다.

활성 상태의 Activity이므로 변경사항은 Observer에 전달된다.

Observer는 변경사항을 수신해서 변경된 데이터로 UI를 갱신한다.

==

LiveData 객체와 Observer 객체를 만든다.

LiveData 객체에 Observer 객체를 연결하기 위해 LiveData의 observer 함수에 첫 번째 인자로 LifecycleOwner를 가진 현재 Activity를 넣고, 두 번째 인자로 앞서 만든 Observer 객체를 넣는다.

해당 Activity가 활성환된 상태에서 LiveData의 값에 변경이 발생하면 이를 관찰하고 있던 Observer객체가 알림을 받는다.



```kotlin
liveData.observe(this,{
  button.text = it
})
```





----

참고 사이트

brunch - https://brunch.co.kr/@mystoryg/152

공홈 - https://developer.android.com/topic/libraries/architecture/livedata