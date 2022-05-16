# Moshi

> JSON과 객체 사이의 직렬화, 역직렬화를 쉽고 안전하게 할 수 있도록 돋는 라이브러리

통신에서 객체 그 자체를 통신에 사용하기 어렵다.

컴퓨터에서는 파일을 다른 컴퓨터로 보내기 전 통신이 가능하면서 나중에 재구성할 수 있는 포맷으로 변환해주어야 한다.

이 과정을 직렬화 라고 한다.

이렇게 변환된 포멧의 일종이 바로 JSON 이다.

직렬화된 파일은 다시 객체 형태로 변환되어야 하는데 이러한 과정을 바로 역직렬화라고 한다.



Moshi는 JSON과 객체 사이의 직렬화, 역직렬화를 쉽고 안전하게 할 수 있도록 돋는 라이브러리이다.

Moshi의 특징으로는 리플랙션과 Codegen 방식의 변환을 모두 지원한다는 점이다.



## Moshi 사용

Moshi 사용에 앞서 어떻게 JSON 구조의 맞춰 Moshi Annotation들을 대응시켜야 하는지 알아야 한다.

Moshi에서 JSON 구조에 대해 사용할 수 있는 Annotation은 두가지이다.

- `@JsonClass`
- `@field:Json`

![image-20220512173052973](https://tva1.sinaimg.cn/large/e6c9d24egy1h25pdd9uetj20xc0niwht.jpg)



### `@JsonClass` 

`@JsonClass` 는 JSON Object에 대응하는 역할을 한다.

따라서 `@JsonClass` 는 Json Object에 대응되는 class를 만들 때 그 위에 붙인다.

ex)

```kotlin
@JsonClass(generateAdapter = true)
data class PokemonList()
```

`@JsonClass` 에는 파라미터로 generateAdapter가 있는데 이것을 true로 해주어야 codegen 방식으로 직렬화, 역직렬화가 가능해진다.



### `@field:Json`

`@field:Json` 은 JSON 내부에 Key-Value 값에 붙인다.

`@field:Json` 은 name 이라는 파라미터를 가지는데 이 파라미터는 JSON 포맷의 key값에 대응되도록 만들어야 한다.

위 그림은 key-value 쌍들은 다음과 같이 표현 가능하다.

```kotlin
@JsonClass(generateAdapter = true)
data class PokemonList(
	@field:Json(name = "count") val count : Int,
  @field:Json(name = "next") val next : String,
  @field:Json(name = "previous") val previous: String
)
```



---

참고 사이트

코틀린월드 : https://kotlinworld.com/117?category=921149





## Moshi 적용



### Built-in Type Adapters

>  moshi는 Build-in 으로 Kotlin의 주요 기능들을 일거나 쓸 수 있게 도와준다.

- Primitives (int, float, char ...)
- Arrays, Collections, Lists, Sets, Maps
- String
- Enums



### Custom Type Adapter

> moshi 의 특징으로 `@ToJson`, `@FromJson` 을 지정해서 원하는 클래스의 Adapter를 원하는 대로 구성 할 수 있다.

```kotlin
class CardAdapter{
  @ToJson 
  fun toJson(card : Card) : String {
  	return "${card.rank} ${card.suit.name().substring(0,1)}"  
  }
  
  @FromJson
  fun fromJson(card : String) : Card {
    if(card.length() != 2) throw JsonDataException("Unknown card : $card")
    
    val rank = card[0]
    return when (card[1]) {
      'C' -> Card(rank, Suit.CLUBS)
      'D' -> Card(rank, Suit.DIAMONDS)
      'H' -> Card(rank, Suit.HEARTS)
      'S' -> Card(rank, Suit.SPADES)
      else -> throw JsonDataException("unknown suit: $card")
    }
  }
}

val moshi = Moshi.Builder()
		.add(CardAdpater())
		.build()
```

또다른 예시 링크 : https://gist.github.com/swankjesse/61354fd0a20bf56072f6a1d0c82fb9fc



### Adapter convenience methods

> moshi Adapter에서는 편의성을 위한 함수들이 여러가지 존재한다.

- nullSafe() : Adpater에서 제공하는 fromJson / toJson에 대한 결과가 null을 허용해준다. 기본적으로 nullsafe
- nonNull() : Adapter에서 제공하는 fromJson / toJson에 대한 결과가 null을 허용하지 않는다.
- lenient() : Json의 형식을 느슨하게 바꿔준다. (???)
- indent() : `adapter.toJson()` 호출 할 때 읽기 쉽게 indent를 추가
- serializeNulls() : `adpater.toJson()` 호출 할 때 null도 직렬화가 된다.



### Fails Gracefully

일반적으로 Reflection과 달리 Moshi는 일이 잘못 될 때 도움을 주기 위해 고안되어 JSON 문서를 읽는 중 오류가 발생하거나 형식이 잘못된 경우 java.io.IOException을 발생키시고, 타입 포맷과 일치하지 않으면 JsonDataException이 발생한다.



### Custom field name

Moshi는 @Json(name = "name")을 활용해서 json에 이름과 변수의 이름을 다르게 설정하고 매핑 할 수 있다.

`@Json(name = "closed_issues") val closedCount : Long`



### @JsonQualifier

커스텀 어댑터를 사용할 때 같은 타입의 값에 대한 특별한 처리를 하고 싶을 때 @JsonQualifierf를 활용하면 된다.



아래와 같이 colorItem에 다 같은 int 값인데 다르게 처리하고 싶을 때 CustomAdapter를 만들어서 Moshi빌드 할 때 넣어주고 adapter를 빼면 된다.

여기서 헷갈릴 수 있는 게 CustomAdapter 가 ColorItem에 대한 encoding이나 decoding을 해주는 것이 아니고 하나의 어노테이션에 대해서만 특별하게 처리해준다.

```kotlin
@JsonClass(generateAdapter = true)
data class ColorItem(val width : Int, val height : Int, @HexColor val color : Int)

class CustomAdapter{
    @ToJson
    fun toJson(@HexColor rgb: Int): String? {
        return String.format("#%06x", rgb)
    }

    @FromJson
    @HexColor
    fun fromJson(rgb: String): Int {
        return rgb.substring(1).toInt(16)
    }
}

//사용예시
val moshi = Moshi.Builder().add(CustomAdpater()).build()
val itemAdapter = moshi.adapter(ColorItem::class.java)
val item = itemAdapter.fromJson(colorJson)
```



### @transient

특정 모델들은 데이터를 json에 포함시키고 싶지 않은 값들이 있을 수 있다.

이럴 때 @Transient 를 활용하면 된다.

fromJson , toJson 할 때 해당 값을 Skip 하게 되고 만약 @Transient 어노테이션이 있음에도 불구하고 Json에 해당 값이 있다면 크래시가 난다.

그래서 @Transient 가 달렸다면 반드시 default Value를 선언해줘야 한다.





-----

참고 사이트

깃험 : https://colinch4.github.io/2020-12-03/android_moshi/

moshi github : https://github.com/square/moshi#reflection