# ViewModel

> UI 관련 데이터를 저장하고 관리해주는 역할

ViewModel이란 Android Jetpack의 구성요소 중 하나,

본래 ViewModel이란 이름은 소프트웨어 개발 디자인 패턴 중 하나인 MVVM(Model - View - ViewModel) 디자인 패턴으로부터 파생



MVVM 관점에서 ViewModel과 Android Jetpack의 ViewModel 클래스를 구분하기 위해 ViewModel 앞에 AAC를 붙인다.

AAC - Android Architecture Componets

## ViewModel이 필요한 이유

MVVM의 관점에서 ViewModel은 View로부터 독립적이며, View가 필요로 하는 데이터만을 소유한다.



안드로이드 앱 개발시에도 MVVM 디자인 패턴을 적용하면 Activity나 Fragment 같은 UI 컨트롤러의 과도한 책임을 분담하여 클래스가 거대해지는 것을 방지하고, 유지보수, 재사용성, 테스트 등을 용이 하게 만들어 준다.



구글에서도 MVVM 패턴을 권장하고 있으며, MVVM 관점의 ViewModel을 구현 할 때, AAC ViewModel을 사용하면 좋다.



## ViewModel의 특징

ViewModel은 Activity에서는 Activity가 완전히 종료될 때 까지, 그리고 Fragment에서는 Fragment가 분리될 때까지 메모리에 남아있도록 설계되어 있다.

![Activity와 ViewModel의 생명주기 비교](https://miro.medium.com/max/1044/0*k7fy3R0bG1UjhTvm.png)

위 그림 보면 Activity 생명주기와 ViewModel 생명주기를 함께 확인 할 수 잇다.

Activity가 최초 생성 될 때 일반적으로 ViewModel을 인스턴스화 하여 생명주기를 함께 시작한다.

(Fragment 생명주기와 함께하도록도 가능)



Configuration(ex - 화면 회전)을 회전 시키면 액티비티가 다시 시작된다.

하지만 ViewModel은 여전히 메모리 상에 남아 있는다.

이는 Activity 내부에서 Configuration 변경과 무관하게 유지되는 NonConfigurationInstances 객체를 따로 관리하기 때문이다.



Activity의 finish() 호출등에 의해 생명주기가 종료됨에 따라 내부의 LifecycleEventObserver를 통해 ViewModel도 onCleared() 콜백 메서드를 호출하고 종료된다.



## ViewModel 요청 프로세스

![img](https://miro.medium.com/max/1400/0*Io9CAKKPaZbZH1Q0.png)

1. ViewModelProvider를 통해 ViewModel 인스턴스를 요청한다.
2. ViewModelProvider 내부에서는 ViewModelStoreOwner를 참조하여 ViewModelStore 를 가져온다.
3. ViewModelStore에게 이미 생성된 ViewModel 인스턴스를 요청한다.
4. 만약 ViewModelStore가 ViewModel 인스턴스를 가지고 있지 않다면, Factory를 통해 ViewModel 인스턴스를 생성한다.
5. 생성한 ViewModel 인스턴스를 ViewModelStore에 저장하고 만들어진 ViewModel 인스턴스를 클라이언트에게 반환한다.



## ViewModel 구현



ViewModel 클래스를 상속하는 클래스 정의

```kotlin
class MainViewModel : ViewModel(){
  //...
}
```



ViewModel 인스턴스 접근

```kotlin
class MainActivity : AppCompatActivity(){
  lateinit var viewModel : MainViewModel
  
  override fun onCreate(savedInstanceState: Bundle?){
    super.onCreate(savedInstanceState)
    
    viewModel = ViewModelProvider(
      this,
      ViewModelProvider.NewInstanceFactory()).get(MainViewModel::class.java)
  }
}
```

ViewModel 생성을 위해서는 ViewModelProvider 필요

ViewModelProvider 생성을 위한 생성자 매개변수로 ViewModelStoreOwner, ViewModelProvider.Factory가 필요



Activity가 ViewModelStoreOwner 인터페이스를 구현하고 있다.

AppCompatActivity를 사용하고 있다면 별도로 ViewModelStoreOwner를 구현할 필요 없다.

Fragment 또한 ViewModelStoreOwner를 구현하고 있다.

ViewModel의 생명주기를 Fragment와 함께 하길 원하는 경우 ViewModelProvider의 생성자 첫번째 인자로 ViewModelStoreOwner 지정 시 Fragment를 넘겨주면 된다.



ViewModelProvider.Factory의 경우 ViewModelProvider 내에 선언되어 있다.

생성자가 없는 ViewModel을 인스턴스화 하는 경우, NewInstanceFactory 클래스를 사용하고, 그 외에는 ViewModelProvider.Factory를 구현해야 한다.



Application을 멤버로 갖는 AndroidViewModel 이라는 클래스가 이미 ViewModelProvider에 정의되어 있다.

클래스 사용하는 경우

```kotlin
viewModel = ViewModelProvider(
  this,            			ViewModelProvider.AndroidViewModelFactory(application)).get(MainViewModel::class.java)

```



## ViewModel을 이용한 Fragment 간 Data 공유

![img](https://miro.medium.com/max/1400/0*dKrzJEZg_wtFXrqB.png)

위 그림은 SharedViewModel에 두개의 Fragment가 동시에 접근하고 있다.

SharedViewModel을 인스턴스를 얻을 때 ViewModelProvider의 첫번째 생성자 매개 변수인 ViewModelStoreOwner를 Fragment들이 속해 있는 부모 Activity로 지정하자.

그러면 각 Fragment는 동일한 SharedViewModel 인스턴스를 얻게 된다.

SharedViewModel의 생명주기는 Activity를 따르고, Fragment의 생명주기는 Activity의 서브셋이므로 Fragment의 생명주기 동안에 자유롭게 데이터를 공유 할 수 있다.



```kotlin
class SharedViewModel : ViewModel(){
  //...
}

class MasterFragment : Fragment(){
  lateinit var viewModel : SharedViewModel
  
  override fun onCreate(savedInstanceState : Bundle?){
    super.onCreate(savedInstanceState)
    activity?.run{
      viewModel = ViewModelProvider(this, ViewModelProvider.NewInstanceFactory()).get(SharedViewModel::class.java)
    }
  }
}

class DetailFragment : Fragment(){
    lateinit var viewModel : SharedViewModel
  
  override fun onCreate(savedInstanceState : Bundle?){
    super.onCreate(savedInstanceState)
    activity?.run{
      viewModel = ViewModelProvider(this, ViewModelProvider.NewInstanceFactory()).get(SharedViewModel::class.java)
    }
  }
}
```

Activity 범위 내에서 ViewModel을 공유하여 Fragment가 데이터를 공유 했을 때 장점

- Activity는 Fragment 커뮤니케이션에 개입하지 않아도 된다.
- Fragment는 SharedViewModel 외에 다른 부분에 대해서 의존하지 않아도 된다.
  - Fragment 중 하나가 사라져도 다른 Fragment는 평소처럼 동족한다.

- 각 Fragment는 자체 생명주기를 가지고 있고, 다른 Fragment의 생명주기에 영향을 주거나 받지 않는다.



---

참고사이트

medium - https://charlezz.medium.com/viewmodel%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-viewmodel-%EC%B4%88%EB%B3%B4%EB%A5%BC-%EC%9C%84%ED%95%9C-%EA%B0%80%EC%9D%B4%EB%93%9C-e1be5dc1ac18



tistory - https://todaycode.tistory.com/33?category=979455



공홈 - https://developer.android.com/topic/libraries/architecture/viewmodel?hl=ko