# Fragment

> Activity는 User와 App이 상호작용하는 진입점이며, 하나의 화면을 구성하는 것이고, Fragment는 Activity 안에서 화면의 일부를 구성하는 것이다.

> 부분 화면



## 주요특징

- Activity는 User와 App이 상호작용하는 진입점이며, 하나의 화면을 구성하는 것이고, Fragment는 Activity 안에서 화면의 일부를 구성하는 것이다.
-  App의 전체 UI에서 어딘가에 반복적으로 재사용 가능한 부분을 말한다.
- 단일 화면이나 화면 일부에 관한 UI를 정의하는데 적합하다.



- 자체 레이아웃(xml 파일을 정의할수  있음)을 가질 수 있고 자체 생명 주기를 보유한다.
- 입력 이벤트를 받아 처리 할 수 있다.



- 독립적으로 존재할 수 없고, 반드시 Activity 나 다른 Fragment에 호스팅 되어야 한다.
  - Fragment를 갖고 있는 Activity or Fragment를 Host Activity, Host Fragment 라고 한다.
- Activity 내에서 실행 중 추가, 제거 가능하다.
- Fragment의 view 계층 구조는 Host view 계층 구조의 일부가 된다.



- Android Jetpack 라이브러리 중 Navigation, BottomNavigationView, ViewPager2 등 Fragment와 호환되도록 설계 되어 Fragment가 해당 라이브러리와 자주 사용된다.



## 재사용성

프래그먼트의 존재 이유는 UI 중에서 재사용 가능한 부분을 재사용 하기 위함이다.



프래그먼트는 자체 UI를 개별적인 청크로써 사용할 수 있다.

개별 청크 단위로 다른 곳에서 재사용할 수 있다.



청크 - 멀티미디어 파일 형식에서 사용되는 정보 조각



## Activity와 Fragment 목적성

- Activity : App 전체적인 UI에 포함될 요소들을 배치하는 곳
- Fragment : 단일 화면, 화면 일부에 관한 UI를 정의하는데 적합

![02](md-images/106560262-1947b280-656a-11eb-8c72-38207d027062.png)

하늘색 박스 부분은 Activity에 배치된 NavigationView

연두색 박스 부분은 Fragment 자체 UI

Activity는 Fragment를 호스팅 하고 있다.



## Fragment 장점

- App의 단일화면 이나 부분 화면 을 Fragment로 구현해서 실행 시 UI 모습을 사용자와 상호작용하면서 실시간으로 수정 할 수 있다.
  - Fragment 추가/교체/삭제 등의 작업이 실행됨으로 화면이 바뀌는 것처럼 보임
- Fragment를 사용하여 런타임 동안 UI를 실시간으로 바꿀 시 Host Activity의 수명 주기가 STARTED 상태 이상에 있는 동안에만 가능하다.
- 런타임 동안 Fragment의 변경사항(추가/교체/삭제)이 발생하면 FragmentManager 가 관리하는 Fragment BackStack에 변경 사항 히스토리를 저장하여 기록할 수 있다.
- Fragment BackStack에 저장된 변경 사항들에 한에서 사용자 '뒤로' 버튼을 눌렀을 경우 '변경 사항 취소'가 되어 '되돌리기'  작업을 진행할 수 있다.



## Fragment 주의점

- Fragment는 재사용 가능한 자체 UI를 가지기 때문에 어느 Activity 에나 호스팅 될 수 있고 어느 Fragment에나 호스팅 될 수 있다.

- 따라서, Fragment Class에는 자체 UI를 관리하는 로직만 구현해야 하고 다른 Activity 나 Fragment를 직접 조작하는 로직을 포함해서는 안된다. (모듈성, 재사용성을 해치게 됨)

  = Fragment가 다른 Activity, Fragment에 의존하면 안된다.



## Fragment Class 만드는 방법

> 프래그먼트를 어떻게 만드는지,
>
> 어떻게 Acitivity에 호스팅 시키는지



### 환경설정

> 프래그먼트 클래스를 만들기전, 안드로이드 프로젝트에 Jetpack Fragment 라이브러리 종속성을 추가

Android 라이브러리는 Google Maven Repository 에서 찾을 수 있으므로 build.gradle 파일(프로젝트 수준)에 Google Maven 저장소를 먼저 추가해야 한다

```xml
  // build.gradle(Project 수준) 파일
  buildscript {
      ...

      repositories {
          google()
          ...
      }
  }

  allprojects {
      repositories {
          google()
          ...
      }
  }
```



프로젝트에 Google Maven 저장소를 추가했다면 build.gradle 파일(app 수준)에 JetPack Fragment 라이브러리 종속성을 추가하면 된다.

```xml
  // build.gradle(App 수준) 파일
  dependencies {
      // 최신 버전 대입
      def fragment_version = "1.2.5"

      // Java language implementation(개발 언어 별 선택)
      implementation "androidx.fragment:fragment:$fragment_version"
      // Kotlin(개발 언어 별 선택)
      implementation "androidx.fragment:fragment-ktx:$fragment_version"
  }
```



### 클래스 생성

- 프래그먼트 클래스를 생성한다는 것은 해당 프래그먼트의 자체 UI 조작을 담당하는 클래스를 생성한다는 것

- 프래그먼트 클래스는 위 과정에서 추가한 Jetpack Fragment 라이브러리에서 제공하는 Fragment 클래스를 상속하는 클래스를 상속하는 클래스를 구현함으로 생성

  ![03](https://user-images.githubusercontent.com/31889335/106571021-0806a200-657a-11eb-8949-ea9bbd814097.png)

- 프래그먼트 클래스의 생성자 함수는 두 가지 종류가 있다. 하나는 default 생성자, 다른 하나는 default 생성자 대안책

- 대안책으로 사용할 수 있는 생성자 함수를 사용하여 프래그먼트 클래스의 인스턴스를 생성할 경우 장점이 존재하다. Default 생성자 함수를 사용하여 프래그먼트 클래스의 인스턴스를 생성하는 경우에는 클래스 내부에 onCreateView(LayoutInflater, ViewGroup, Bundle) 메소드를 구현해야 한다. 하지만 대안책으로 사용할 수 있는 생성자 함수를 사용하여 onCreateView() 구현을 생략할 수 있다.

  - onCreateView() 메소드 내에서 프래그먼트 자체 UI 레이아웃을 inflate 하는 코드를 작성한다.

    ​	inflate 하다 = xml 파일에 정의된 뷰들을 메모리에 객체화하는 것

    ​	즉, xml 파일에 정의된 뷰들을 객체화해서 코드에서 사용하기 위함이다.

    Inflate 하여 뷰들을 객체화한 후에야 클래스 내부의 코드에서 사용할 수 있으므로 프래그먼트 클래스와 프래그먼트 자체 UI를 붙인다 라고 이해

- 프래그먼트 코드

  ```kotlin
  import androidx.fragment.app.Fragment
  
  class ExampleFragment : Fragment(){
    // TODO (onCreateView 메소드 구현)
  }
  
  class ExampleFragment2 : Fragment(R.layout.example_fragment){
    // Alternate 생성자 함수 사용
    // 생성자 함수 인자로 프래그먼트 자체 UI 레이아웃이 정의된 xml 파일의 id값을 전달
  }
  ```

  이렇게 생성한 클래스 내에서 상속한 Fragment 클래스에 포함된 메소드들을 override 하여 해당 프래그먼트가 담당할 로직들을 구현



### Activity에 프래그먼트 호스팅

- 프래그먼트는 독립적으로 존재할 수 없고, 반드시 Activity 나 다른 프래그먼트에 호스팅 되어야 한다.

- 프래그먼트를 Activity에 호스팅 시키려면 해당 Activity가 FragmentAcitivity를 상속하는 Activity 여야 한다. 그래야 해당 Activity 레이아웃 일부분에 프래그먼트 자체 UI를 배치할 수 있다.

- 만약 Activitiy가 이미 AppCompatActivitiy 클래스를 상속하고 있다면 FragmentActivity를 따로 상속할 필요가 없다.

  ![26](https://user-images.githubusercontent.com/31889335/106901334-f6b8c380-673a-11eb-9e8f-4d7b9b675cae.png)

​		해당 클래스는 이미 FragmentActivity를 상속하는 클래스 이기 때문이다.

- 호스팅 시키는 2가지 방법
  - Activity UI 레이아웃 안에 프래그먼트 존재를 정의하여 Activity UI가 Activity 클래스에 inflate 될 때 프래그먼트 자체 UI도 자동으로 프래그먼트 클래스에 inflate 시키는 방법
  - Activity UI 레이아웃 안에 프래그먼트 컨테이너 (= 이 위치에 프래그먼트 자체 UI가 배치 될 것이라고 위치를 지정해두는 것)를 정의하고 프로그래밍적으로 해당 컨테이너 안에 프래그먼트를 추가 하는 방법
- 2가지 방법 모두 Activitiy UI 레이아웃 안에 FragmentContainerView를 정의함으로써 해당 프래그먼트가 배치될 위치를 정의해야 한다는 점
- FragmentContainerView는 FrameLayout에서 제공하지 않는 조작 관련 사항이 추가된 새로운 뷰이다. 따라서 Activity UI 레이아웃 내에서 프래그먼트 자체 UI 위치를 지정할 때는 FragmentContainerView를 사용하는 것이 권장사항



#### 첫번째 방법 (Activity UI가 inflate 될 때 프래그먼트 UI도 inflate)

```xml
  <!-- activity_main.xml 파일 -->
  <androidx.constraintlayout.widget.ConstraintLayout 
      ...>

      <!-- FragmentContainerView 배치 -->
      <androidx.fragment.app.FragmentContainerView
          android:id="@+id/fragment_container_view"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          app:layout_constraintTop_toTopOf="parent"
          app:layout_constraintStart_toStartOf="parent"
          app:layout_constraintBottom_toBottomOf="parent"
          app:layout_constraintEnd_toEndOf="parent"
          android:name="com.example.fragment.ExampleFragment"/>

  </androidx.constraintlayout.widget.ConstraintLayout>
```

- FragmentContainerVeiw 내 android:name 속성을 통해 FragmentContainerView에 추가될 프래그먼트 클래스를 지정, 이 속성을 지정 시 자동으로 일련의 내부 동작이 실행된다.
- Android:name 속성의 내부 동작은 Acitivity UI 레이아웃이 inflate되는 과정에서 FragmentContainerView가 inflate 될 때 자동으로 android:name 속성에 지정된 프래그먼트를 새로운 인스턴스로 생성
- 지정된 프래그먼트의 인스턴스가 생성되면 프래그먼트 수명 주기 메소드 중 onInflate() 메소드가 호출된다. 이 메소드 호출된 후, 해당 프래그먼트 자체 UI를 FragmentContainerView 위치에 추가하는 FragmentTransaction이 실행



#### 두번째 방법 (Activity UI 레이아웃 안 Container 안에 프래그먼트 추가)

```xml
  <!-- activity_main.xml 파일 -->
  <androidx.constraintlayout.widget.ConstraintLayout
      ...>

      <!-- FragmentContainerView 배치 -->
      <androidx.fragment.app.FragmentContainerView
      android:id="@+id/fragment_container_view"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      app:layout_constraintTop_toTopOf="parent"
      app:layout_constraintStart_toStartOf="parent"
      app:layout_constraintBottom_toBottomOf="parent"
      app:layout_constraintEnd_toEndOf="parent" />

  </androidx.constraintlayout.widget.ConstraintLayout>
```

- android:name 속성 지정하지 않음으로써 Activity UI 레이아웃 안에서 프래그먼트 컨테이너의 위치만 배치
- 두번째 방법 사용 시 Activity가 running 상태일 동안 프래그먼트 컨테이너 안에 프로그래밍적으로 프래그먼트를 추가/교체/삭제
- 프로그램이적으로 프래그먼트를 추가/교체/삭제하려면 프래그먼트를 호스팅하는 Activitiy 내에서 FragmentManager 클래스를 인스턴스화해야한다.
- FragmentManager를 인스턴스화 하는 이유는 프래그먼트 추가/교체/삭제 작업을 제공하는 FragmentTransaction 클래스를 사용하기 위함이다.
- FragmentManager 클래스를 인스턴스화 하는 방법은 FragmentActivitiy 클래스에 제공되는 getSupportFragmentManager() 함수 호출
- FragmentManager 인스턴스화 한 후, Acitivity의 onCreate() 메소드내에서 FragmentTransaction 클래스가 제공하는 add() 함수를 호출하여 프래그먼트 컨테이너 위치에 추가하고자 할 프래그먼트 자체 UI를 추가



## FragmentManager

> 백 스택(Back Stack)에 프래그먼트 추가/교체/삭제 작업에 의한 변경 사항을 push, pop 작업을 담당하는 클래스



- 프래그먼트를 관리하는 더 좋은 방법 -> JetPack Navigation 라이브러리 사용하면 FragmentManager를 직접 다뤄야 할 일이 적어져 편하다.

- Navigation 라이브러리 내부 동작에 FragmentManager를 사용한 작업이 구현되어 있기 때문이다.

- FragmentManager가 중요한 이유

  - Navigation 라이브러리도 결국 내부 동작에 FragmentManager를 사용할 만큼 프래그먼트를 사용하는 앱에서는 FragmentManager 사용이 빈번하다.
  - 그러므로 FragmentManager가 무엇이고, 어떻게 작동하는지 파악하는 것이 중요

- FragmentManager 인스턴스화

  - 런타임 시에 프래그먼트 추가/삭제/교체 작업 등을 하려면 먼저 FragmentManager 클래스를 인스턴스화 해야 한다.
  - 인스턴스화 방법은 2가지 경우에 따라 다르다.
    - Activity 클래스에서 인스턴스화하는 경우
    - Fragment 클래스에서 인스턴스화하는 경우
  - Activity 클래스에서 FragmentManager 인스턴스화
    - FragmentActivitiy 클래스를 상속하는 모든 서브 클래스에서 getSupportFragmentManager() 함수 호출
  - Fragment 클래스에서 FragmentManager 인스턴스화
    - 프래그먼트는 Acitivity에 호스팅되거나 다른 프래그먼트에 호스팅 되어야 한다.
    - 프래그먼트가 하위 프래그먼트를 호스팅하는 경우 호스트 프래그먼트 내에서 getChildFragmentManager() 함수를 호출하여 하위 프래그먼트에 관한 FragmentManager 를 인스턴스화
    - 하위 프래그먼트에서 호스트 프래그먼트를 관리하는 FragmentManager를 인스턴스화를 getParentFragmentManager() 함수 호출

- Fragment와 FragmentManager 관계

  ![07](https://user-images.githubusercontent.com/31889335/106621524-fe9b2b00-65b5-11eb-95f6-753e36591a57.png)

  - 연두색으로 표시된 Activity는 하늘색으로 표시된 프래그먼트의 호스트 Activity 이다. 이 Activity에는 BottomNavigationView가 배치되어 있고 Home/Profile 탭 변경 시 하늘색으로 표시된 프래그먼트를 교체하여 사용자에게 앱 탐색 경험을 제공한다.
  - Example1 에서 하늘색으로 표시된 프래그먼트는 하위 프래그먼트 2개를 호스팅하는 호스트 프래그먼트 이다. 하위 프래그먼트 2개를 통해 화면을 분할하고 있다.
  - Example2 에서 하늘색으로 표시된 프래그먼트는 ViewPager2(스와이프 뷰)를 구현하는 하위 프래그먼트 1개를 호스팅 하는 프래그먼트 이다.
  - ![08](https://user-images.githubusercontent.com/31889335/106622897-6aca5e80-65b7-11eb-8721-791aa97b9b67.png)

- 인스턴스화된 FragmentManager 사용

  - FragmentManager 는 프래그먼트 추가/교체/삭제 작업을 제공하는 FragmentTransaction 클래스를 구현
  - ![29](https://user-images.githubusercontent.com/31889335/106918436-8c108380-674c-11eb-8d3f-3b7030df4a7d.png)

  - supporFragmentManager를 호출하여 FragmentManager 인스턴스하고 commit{} 블록을 호출한 코드
  - commit{} 블로 안에 프래그먼트 추가/교체/삭제 작업을 명시

- FragmentManager 사용 - 기존 프래그먼트 찾기

  - FragmentManager가 제공하는 findFragmentById() 함수를 사용하여 프래그먼트 컨테이너 내의 현재 프래그먼트 참조를 가져올 수 있다.
  - 또는 프래그먼트에 고유한 태그를 부여한 후 findFragmentByTag() 함수를 호출하여 해당 태그가 부여된 프래그먼트 참조를 가져올 수 있다.
  - findFragmentById()는 UI를 제공하는 프래그먼트를 참조할 때 사용하고 findFragmenyByTag() 는 UI를 제공하지 않는 프래그먼트를 참조할 때 사용한다.

- 하위 프래그먼트

  - 일반적으로 안드로이드 앱은 단일 or 소수의 Activitiy로 구성하는 것이 좋다.
  - 주로 Activity에는 최상위 탐색 뷰 (Navigation 같은 뷰)가 배치된다.
  - 이 외의 Activitiy를 구성하는 개별 UI는 프래그먼트를 사용하여 구현한다.
  - 여러 프래그먼트를 한 번에 하나의 Activity에 표시하려면 분할 뷰 프래그먼트와 하위 프래그먼트를 사용한다.
  - ![12](https://user-images.githubusercontent.com/31889335/106687938-7433e500-6610-11eb-8edc-024ac33fd1df.png)

  - 하늘색으로 표시된 부분이 분할 뷰 프래그먼트이고 하얀색으로 표시된 부분이 하위 프래그먼트 2개

- 하위 프래그먼트 고려사항

  - 특정 시점에서 프래그먼트 백 스택을 제어하는 데에는 프래그먼트 관리자 1개만 허용된다.
  - 앱에서 여러 하위 프래그먼트를 동시에 화면에 표시하거나 사용하려는 경우 프래그먼트 관리자 한 개를 지정하여 앱의 기본 탐색을 처리해야 한다.
  - 앱의 기본 탐색이란
    - Activity를 탐색의 가장 바깥쪽 레이어로 간주하고 하위 프래그먼트의 각 레이어를 바깥쪽 레이어 아래에 매핑한다. 이 때 각 레이어에는 기본 탐색 프래그먼트가 하나씩 있어야 한다. '뒤로' 이벤트가 발생하면 가장 먼저 가장 안쪽 레이어에서 탐색 동작을 컨트롤한다. '뒤로' 이벤트가 발생했을 때 가장 안쪽 레이어에 다시 표시할 프래그먼트 트랜잭션이 더 이상 없으면 탐색 컨트롤은 다음 바깥 레이어로 옮겨진다. 이 프로세스는 Activity에 도달할 때 까지 반복된다.
  - 프래그먼트 트랜잭션 내에서 기본 탐색 프래그먼트를 지정하려면 FragmentTransaction 클래스에 제공하는 setPrimaryNavigationFragment() 함수를 호출하여 childFragmentManager에 기본 컨트롤이 있어야 하는 프래그먼트의 인스턴스를 전달한다.





## LifeCycle

> 화면을 보이는 것을 기준으로 생명 주기 메서드를 가지는데
>
> 생성 관련 5개, 소멸 관련 5개를 가지고 있다.

![fragmentlifecycle](md-images/fragmentLifeCycle.png)

**생명 주기**

생성과 관련된 메서드가 5개나 이지만

프래그먼트를 포함하고 있는 액티비티가 화면에 계속 나타나고 있는 상태에서 onAttach() 부토 onResume() 까지 메서드가 모두 한번에 호출된다.

1. onAttach()

   프래그먼트 매니저를 통해 액티비티에 프래그먼트가 추가되고 commit 되는 순간 호출

2. onCreate()

   프래그먼트가 생성됨과 동시에 호출, UI인 뷰와 관련된 것을 제외한 프래그먼트 자원(주로 변수)을 초기화 할 때 사용

3. onCreateView()

   UI와 관련된 뷰를 초기화

4. onStart()

   프래그먼트가 새로 추가 되거나

   화면에서 사라졌다가 다시 나타나면 onCreateView()는 호출되지 않고 onStart()만 호출

   주로 화면 생성 후 화면에 입력될 값을 초기화 하는 용도

5. onResume()

   onStart()와 같은 용도로 사용

   다른 점은 소멸 주기 메서드가 onPause() 상태에서 멈췄을 때(현 프래그먼트 일부가 가려지지 않았을 떄)는 onStart()를 거치지 않고 onResume()이 바로 호출

**소멸 주기**

현재 프래그먼트 위로 새로운 프래그먼트가 추가 되거나

현재 프래그먼트를 제거하면 소멸 주기와 관련된 메서드가 순차적으로 호출된다.

1. onPause()

   현재 프래그먼트가 화면에서 사라지면 호출

   주로 동영상 플레이어를 일시정지하거나 현재 작업을 잠시 멈추는 용도로 사용

2. onStop()

   onPause()와 다른 점은 현재 프래그먼트가 화면에 일부분이라도 보이면 onStop()은 호출되지 않는다.

   동영상 플레이를 예로 들면 일시정지가 아닌 정지를 하는 용도

3. onDestoryView()

   뷰의 초기화를 해제하는 용도

   이 메서드 호출된 후 생성 주기 메서드인 onCreateView()에서 인플레이터로 생성한 View가 모두 소멸

4. onDestory()

   액티비티에는 아직 남아있지만 프래그먼트 자체는 소멸

   프래그먼트에 연결된 모든 자원을 해제하는 용도

5. onDetach()

   액티비티에서 연결이 해제

---

참고 사이트

김초희 깃헙 : https://choheeis.github.io/newblog//articles/2021-02/fragment