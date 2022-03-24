# Activity Result API



`startActivityForResult()` 메소드가 deprecated 되있다.

![image-20220324150107673](https://tva1.sinaimg.cn/large/e6c9d24egy1h0kxoggshpj21560dwgo3.jpg)



## `startActivityForResult()` 가 deprecated된 이유

1. AndroidX Acitivity, Fragment에 도입된 Activity Result API 사용을 권장하고 있기 때문에
2. 결과를 얻는 Activity를 실행하는 로직을 사용할 때, 메모리 부족으로 인해 프로세스와 Activity가 사라질 수 있다.



기존 방법은 Activity에서는 `startActivityResult()` 를 통해서 콜백을 등록하고, `onActivityResult` 에서 콜백을 처리했다.

두 메서드가 같은 곳에서 구현을 해야하는데, 메모리 부족으로 제대로 동작을 안 할 수 있다는 것이다.

​	-> Activity가 종료되었다가 다시 생성되었을 때 Activity에게 결과를 기다리는 중임을 다시 알려야 한다.



## 기존 API

**화면전환**

- `startActivity(Intent intent)` : 새로운 Activity 시작 (단방향)
- `startActivityForResult(Intent intent, int requestCode, Bundle options)` : 새로운 Activity 시작 + 결과값 전달 (쌍방향)



**결과 반환 및 전달**

- `setResult(int resultCode)`
  - Activity가 종료되면 해당 메서드를 통하여 데이터를 상위 항목으로 되돌릴 수 있음
  - 새롭게 시작한 Activity의 종료시점에 호출하여 데이터를 상위(Parent)로 보내고 resultCode를 전달해야 함
  - 이 모든 정보는 상위 (Parent)의 `Activity.onActivityResult()` 에 다시 나타난다.
- `onActivityResult(int requestCode, int resultCode, Intent data)`
  - 실행한 Activity가 종료되어 시작한 requestCode, 반환된 resultCode, 추가 데이터를 제공할 때 호출



## 새로운 API

> Activity Result API는 다른 Activity를 실행하는 코드에서 결과 콜백을 분리한다.
>
> Result Callback은 프로세스와 Activity를 다시 생성할 때 사용할 수 있어야 하므로 다른 Activity를 실행하는 로직이 사용자 입력 또는 기타 비즈니스 로직을 기반으로 발생하더라도 Activity가 생성될 때마다 콜백을 무조건 등록해야 한다.



> registerForActivityResult() 는 ActivityResultContract 및 ActivityResultCallback을 가져와서 다른 Activity를 실행하는 데 사용할 ActivityResultLauncher를 반환한다.

`registerForActivityResult()` 메서드는 콜백을 등록하는 역할을 한다.

`ActivityResultLauncher` 와 `registerForActivityResult()` 메서드를 사용하면 Activity가 종료되었다가 다시 생성되어도 결과를 기다리고 있다는 것을 알려줄 수 있다.



새로운 API에서 requestCode가 없는 이유

​	-> 원하는 Acitivty Request마다 registerForActivityResult를 실행하여 Result Callback을 분리해서 구현하므로



```kotlin
lateinit var getResult : ActivityResultLauncher<Intent>

override fun onCreate(savedInstanceState : Bundle?){
  super.onCreate(savedInstanceState)
  ...
  
  getResult = registerForActivityResult(ActivityResultContracts.StartActivityForResult()){
    if (it.resultCode == RESULT_OK){
      // code
    }
  }
  
  binding.btn.setOnclickListener{
    val intent = Intent(...)
    getResult.launch(intent)
  }
}
```

- 람다식의 it은 결과를 나타낸다.
- It.resultCode, it.data 등으로 결과 코드(resultCode)와 데이터(Intent)에 접근할 수 있다.
- launch() 함수로 시작한다.

함수의 인자로 들어가는 함수는 ActivityResultContracts 클래스의 static 함수로,

결과를 받기 위해 Activity를 실행하는 StartActivityForResult() 함수를 인자로 넣는다.



launch()를 통해 이동한 새로운 Activity에서는 기존과 동일하게 setResult() 함수를 그대로 사용하면 된다.



## Fragment에서 Activity Result API 사용



```kotlin
val startForResult = registerForAcitivityResult(ActivityReslutContracts.StartActivityForResult()){
  //code
}

override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        binding = FragmentXXXXBinding.inflate(inflater, container, false)

        return binding.root
}

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
				
  			binding.image.setOnClickListener{
         	val intent = Intent(Intent.ACTION_PICK).setType("image/*")
          startForResult.launch(intent)
        }
}
```

Fragment 생성되기 전 (initialization, onAttach(), onCreate())에 registerForActivityResult를 호출해야 한다.

> For this reason, the Activity Result APIs decouple the result callback from the place it your code where you launch the other activity. As the result callback needs to be available when your process and activtiy are recreated, the callback must be unconditionally registered every time your activity is created, even if the logic of launching the other activity only happens based on user input or other business logic.

> 결과를 얻기 위해 Activiy를 시작 시, 메모리 부족으로 인해 프로세스와 Activity가 소멸될 수 있다. 따라서 Activity Result API는 다른 Activity를 실행하는 코드에서 결과 콜백을 분리한다. 프로세스 및 작업을 다시 생성할 때 결과 콜백을 사용할 수 있어야 하므로 다른 작업을 시작하는 로직이 사용자 입력 또는 다른 비즈니스 로직에 따라서만 발생하더라도 작업이 생성될 때마다 콜백을 무조건 등록해야 한다.



---

참고 사이트

티스토리 : 

https://junyoung-developer.tistory.com/151?category=960204

https://junyoung-developer.tistory.com/159?category=960204