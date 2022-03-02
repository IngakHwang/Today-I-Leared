# RecyclerView

> 사용자가 관리하는 많은 수의 데이터 집합(Data Set)을 개별 아이템 단위로 구성하여 화면에 출력하는 뷰그룹(ViewGroup)이며, 한 화면에 표시되기 힘든 많은 수의 데이터를 스크롤 가능한 리스트로 표시해주는 위젯



리사이클러뷰(RecyclerView)는 리스트뷰 (ListView)와 '사용 목적' 및 '동작 방식'이 매우 유사하다.



리사이클러뷰는 기존의 리스트 형태에 화면 구성에 사용되던 리스트뷰에 "유연함(Flexibility)" 과 "성능(Performance)"를 더한, 리스트뷰의 개선판이다.



Recycler 사전적의미 : 재생처리기, 재활용하는 사람



## 리사이클러뷰(RecyclerView)의 재활용(Recycle)

> 리사이클러뷰는 아이템을 표시하기 위해 생성한 뷰를 재활용 한다.

리스트뷰의 경우, 기본 가이드에 따라 구현했을 때 문제점 중 하나는, 리스트 항목이 갱신될 때마다, 매 번 아이템 뷰를 새로 구성해야 한다는 것이다. 

이는 "많은 수의 데이터 집합을 표시" 하는 데 있어서 성능 저하를 야기 할 수 있는 요인이다.



선택적으로 뷰홀더(ViewHolder) 패턴을 선택적으로 적용하여 성능 저하 문제를 해결 할 순 있지만, 선택적으로 적용 할 수 있는 사항이고, 좀 더 복잡해지면 코드 작성 시 고려해야 할 사항이 많아진다.



리스트뷰의 단점을 참고하여, 리사이클러뷰는 아이템을 표시하기 위해 생성한 뷰를 재활용(recycle)한다.

그리고 이를 위해 기본적으로 뷰홀더(ViewHolder) 패턴을 사용하도록 만들어 놓았다.



뷰홀더가 필수 구현 사항으로 만들어졌다는 말은, 개발자가 직접 뷰홀더 패턴을 적용 시 고민해야 했던 여러 이슈들이 리사이클러뷰 구현 사항에 고려되었다는 것을 의미하고,

특히 리아시클러뷰의 가장 큰 장점인 유연함(Flexibility) 에 대한 고려도 충분히 이루어졌다는 것을 의미한다.



## 리사이클러뷰(RecyclerView)의 유연함(Flexibility)

> 리사이클러뷰의 구현 요소나 구현에 따른 결과물이 쉽게 변경되거나 확장 될 수 있다.

"유연함" 이라는 뜻의 "Flexibility"는, 프로그래밍 분야에서, "구현 요소" or "구현에 따른 결과물"이 쉽게 변경되거나 확장될 수 있음을 의미한다.

리사이클러뷰에 적용하면, "리사이클러뷰의 유연함이란, 리사이클러뷰의 구현 요소 or 구현에 따른 결과물이 쉽게 변경되거나 확장 될 수 있다" 



리스트뷰와 비교를 하면

리스트뷰의 기본 구현 내에서는 아이템들을 수직 방향으로만 나열 할 수 있다. 만약 수평 방향으로 아이템들을 나열 시, 리스트뷰가 아닌 다른 뷰를 사용하거나, 리스트뷰의 기능을 상당 부분 재 구현 해야 한다.

그리고 아이템 뷰를 동적으로 구성하기가 쉽지 않은 한계도 있다.

사용자 선택에 따라 아이템 뷰를 완전히 새로운 형태로 바꾸고자 한다면, 모든 상황에 대한 대응을 어댑터 내에서 개발자가 직접 처리해야 한다.



리사이클러뷰는 위의 단점을 보완하여, 개발자가 쉽게 구현할 수 있도록 만들어준다.

수직 뿐만 아니라 수평 방향으로 아이템들이 나열되게 만들 수 있고, 아이템 뷰의 동적 구성을 용이하게 만들어주며, 이를 런타임에 바꾸게 만들 수도 있다.



이런 특징들이, 리사이클러뷰의 유연함 을 나타내는 장점이다.



## 리사이클러뷰(RecyclerView)를 위한 구성 요소

> 어댑터, 레이아웃 매니저, 뷰홀더

리사이클러뷰는 데이터  목록을 아이템 단위의 뷰로 구성하여 화면에 표시하기 위해 '어댑터(Adapter)'를 사용한다.



화면에 표시될 아이템 뷰들을 수직 방향으로, 일렬로 나열 하는 리스트뷰와 달리

리사이클러뷰는 수평 방향 레이아웃 또는 격자(Grid) 형태의 레이아웃으로도 나타낼 수 있다.

이를 위해 리사이클러뷰에서는 아이템 뷰가 나열되는 형태를 관리하기 위한 요소를 제공하는 데, 이를 '레이아웃 매니저(Layout Manager)' 라고 한다.



레이아웃 매니저가 제공하는 레이아웃 형태로, 어댑터를 통해 만들어진 각 아이템 뷰는 '뷰홀더(ViewHolder)' 객체에 저장되어 화면에 표시되고, 필요에 따라 생성 또는 재활용 된다.

![리사이클러뷰의 구성 요소](https://t1.daumcdn.net/cfile/tistory/997123455C88BD1A01)

*리사이클러뷰의 각 구성 요소 구조 및 처리*



### 리사이클러뷰 (RecyclerView)

> 리사이클러뷰는 사용자 데이터를 리스트 형태로 화면에 표시하는 컨테이너 역할을 수행한다. 

![리사이클러뷰의 구성 요소 - RecyclerView](https://t1.daumcdn.net/cfile/tistory/997F764A5C88C37928)



### 어댑터 (Adpater)

> 리사이클러뷰에 표시될 아이템 뷰를 생성하는 역할
>
> 사용자 데이터 리스트로부터 아이템 뷰를 만드는 것

![리사이클러뷰의 구성 요소 - Adapter](https://t1.daumcdn.net/cfile/tistory/994890405C88BD1A02)



수직 방향으로 아이템을 배치 할 수 있는 리스트뷰와 다르게, 리사이클러뷰는 다양한 형태로 아이템을 배치할 수 있다.

이를 위해, 어댑터에서 아이템 뷰를 생성하기 이전에, 어떤 형태로 배치될 아이템 뷰를 만들 것인지를 결정하는 요소가 레이아웃 매니저 이다.



### 레이아웃 매니저 (LayoutManager)

> 리사이클러뷰가 아이템을 화면에 표시할 때, 아이템 뷰들이 리사이클러뷰 내부에서 배치되는 형태를 관리하는 요소
>
> 더 이상 화면에 표시되지 않는 아이템 뷰를 언제 재활용 할 것인지에 대한 정책도 결정

![리사이클러뷰의 구성 요소 - LayoutManager](https://t1.daumcdn.net/cfile/tistory/999303365C88BD1A31)



안드로이드 SDK에서는 리니어, 그리드, 스태거드 그리드 등 레이아웃매니저 들이 기본으로 제공된다.

- 리니어 (LinearLayoutManager) : 수평(Horizontal) 또는 수직(Vertical)방향, 일렬(Linear) 로 아이템 뷰 배치
- 그리드 (GridLayoutManager) : 바둑판 모양의 격자(Grid) 형태로 아이템 뷰 배치
- 스태거드그리드 (StaggeredGridLayoutManager) : 엇갈림(Staggered) 격자(Grid) 형태로 아이템 뷰 배치

![리사이클러뷰 레이아웃매니저의 종류](https://t1.daumcdn.net/cfile/tistory/99BADD375C88BD1A24)

레이아웃 매니저는 더 이상 화면에 표시되지 않는 아이템 뷰를 언제 재활용 할 것인지에 대한 정책도 결정한다.



### 뷰홀더 (ViewHolder)

> 화면에 표시될 아이템 뷰를 저장하는 객체이다.

![리사이클러뷰의 구성 요소 - ViewHolder](https://t1.daumcdn.net/cfile/tistory/9954BA3F5C88BD1A37)

어댑터에 의해 관리되는데, 필요에 따라 (레이아웃매니저의 아이템 뷰 재활용 정책에 따라) 어댑터에서 생성된다.

미리 생성된 뷰홀더 객체가 있는 경우에는 새로 생성하지 않고 이미 만들어져 있는 뷰홀더를 재활용하는데, 이 때는 단순히 데이터가 뷰홀더의 아이템 뷰에 바인딩 (Binding) 된다.



## 사용법

> emety activity 에서 구현 했음



### 액티비티에 리사이클러뷰 추가

> 기본제공되는 activity_main.xml에서 ConstraintLayout에 RecyclerView 추가하고,
>
> Top, Bottom, Start, End 모두 parent로 지정
>
> Id = recyclerView

```xml
<!-activity_main.xml->

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="409dp"
        android:layout_height="729dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```



### 리사이클러뷰 아이템 뷰 레이아웃 추가

> layout 디렉토리 내 새로운 레이아웃 추가
>
> LinearLayout에 TextView 3개 추가
>
> id = TextNo, textTitle, textDate

```xml
<!-item_recycler.xml->

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="50dp"
    android:gravity="center_vertical"
    android:orientation="horizontal">

    <TextView
        android:id="@+id/textNo"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="01" />

    <TextView
        android:id="@+id/textTitle"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="5"
        android:text="Title" />

    <TextView
        android:id="@+id/textDate"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="3"
        android:text="2022-02-02" />
</LinearLayout>
```



### 데이터 클래스 정의

> 레이아웃에 맞춰 화면에 뿌려질 데이터 클래스 생성
>
> 번호, 타이틀, 날짜 세 종류의 값을 담을 데이터 클래스 생성

```kotlin
data class Memo(var no: Int, var title: String, var timestamp: Long)
```





### 어댑터 구현



리스트뷰의 경우, 어댑터를 사용할 때 안드로이드 SDK에서 제공되는 몇 가지 어댑터 중 하나를 선택하거나, 필요한 경우(커스텀 리스트뷰)에 `BaseAdapter` 클래스를 상속받아 새로운 어댑터를 만들었던 것에 반해

리사이클러뷰에서는 반드시 개발자가 어댑터를 직접 구현해야 한다.

이 때 새로 만드는 어댑터는 `RecyclerView.Adapter` 를 상속하여 구현해야 한다.



`RecyclerView.Adapter` 를 상속받아 새로운 어댑터를 만들 때, 오버라이드가 필요한 메서드

| 메서드                                             | 설명                                                  |
| -------------------------------------------------- | ----------------------------------------------------- |
| onCreateViewHolder(ViewGroup parent, int viewType) | viewType 형태의 아이템 뷰를 위한 뷰홀더 객체 생성     |
| onBindViewHolder(ViewHolder holder, int position)  | position에 해당하는 데이터를 뷰홀더의 아이템뷰에 표시 |
| getItemCount()                                     | 전체 아이템 갯수 리턴                                 |



```kotlin
class CustomAdapter: RecyclerView.Adapter<Holder>() {
    var listData = mutableListOf<Memo>()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): Holder {
        val binding = ItemRecyclerBinding.inflate(LayoutInflater.from(parent.context),parent,false)

        return Holder(binding)
    }

    override fun onBindViewHolder(holder: Holder, position: Int) {
        val memo = listData.get(position)
        holder.setMemo(memo)
    }

    override fun getItemCount(): Int {
        return listData.size
    }

}

class Holder(val binding: ItemRecyclerBinding): RecyclerView.ViewHolder(binding.root) {
    fun setMemo(memo: Memo){
        binding.textNo.text = "${memo.no}"
        binding.textTitle.text = memo.title
        var sdf = SimpleDateFormat("yyyy/MM/dd")
        var formattedDate = sdf.format(memo.timestamp)
        binding.textDate.text = formattedDate
    }

}
```



어댑터 내에서 뷰홀더를 위한 클래스를 구현했다.

`RecyclerView.ViewHolder` 를 상속



이렇게 작성한 뷰홀더는 어댑터의 `onCreateViewHolder()` 와 `onBindViewHolder()` 메서드를 통해 각각 생성 및 바인딩 (데이터 표시)되어 화면에 표시된다.



### 리사이클러뷰에 어댑터, 레이아웃 매니저 지정



```kotlin
class MainActivity : AppCompatActivity() {
    val binding by lazy {ActivityMainBinding.inflate(layoutInflater)}

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)

        val data:MutableList<Memo> = loadData()
        var adapter = CustomAdapter()
        adapter.listData = data

        binding.recyclerView.adapter = adapter

        binding.recyclerView.layoutManager = LinearLayoutManager(this)

    }

    fun loadData(): MutableList<Memo>{
        val data: MutableList<Memo> = mutableListOf()

        for (no in 0..100){
            val title = "리사이클러뷰 연습 ${no}"
            val date = System.currentTimeMillis()
            var memo = Memo(no,title,date)

            data.add(memo)
        }

        return data
    }

}
```



`loadData()` 를 통해 더미 데이터 1~99개 생성

`binding.recyclerView.adapter` 에 선언한 `adapter` 선언

`binding.recyclerView.layoutManager` 에 `LinearLayoutManager` 선언 



## 예제 실행 화면

![image-20220302173230525](https://tva1.sinaimg.cn/large/e6c9d24egy1gzvmf6wqh7j20le14kq5n.jpg)