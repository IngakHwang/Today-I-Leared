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