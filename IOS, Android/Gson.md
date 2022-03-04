# Gson

> google에서 제공하는 라이브러리로 객체 -> Json, 혹은 Json -> 객체로 변환하는 데 사용
>
> Java나 Kotlin의 기본적인 데이터 객체 뿐 아니라 사용자가 정의한 데이터 클래스도 간편하게 변환이 가능
>
> Json을 Serialize, Deserialize 를 돕는 라이브러리



Serialize 직렬화 : 객체를 전송 가능한 형태로 만드는 것 (객체 -> JSON)

Deserialize 역직렬화 : 전송 가능한 형태를 객체로 만드는 것 (JSON -> 객체)



## Json 이란?

> JavaScript Object Notation
>
> Key - Value 로 이루어진 구조화된 데이터를 표현하기 위한 문자 기반의 표준 포맷

자바 스크리트 뿐만 아니라 여러 프로그래밍에서 사용된다.



### 기본구조

> 하나의 문자열로 구성되며 `"{key : value}"` 의 객체 형태를 기본으로 하고 콤마 `,`로 구분

value에는 문자열, 숫자, 배열, boolean 값과 다른 객체를 포함 할 수 있다. 

```json
{
  "이름" : "홍길동",
  "나이": 30,
  "성별": "나",
 	"주소": "서울특별시 XX구 XX동",
  "특기": ["축구", "수용"],
  "가족관계": {"#": 2, "아버지": "홍아빠", "어머니": "김엄마"}
}
```



### JSONArray vs JSONObject



JSONArray 

- 배열 대괄호로 시작 `[]`, 콤마 `,` 로 값을 구분 

- 배열 구조
- 배열 안에 문자열, 숫자, 배열, 객체 등을 담을 수 있음
- 후에 Index를 이용하여 값을 꺼낼 수 있으므로 순서에 대한 고려를 해주어야 한다.



JSONObject 

- 배열 중괄호로 시작 `{}`, 
- `key-value` 사이의 구분은 콜론 `:` 

- `key-value` 묶음 간의 구분은 콤마 `,` 
- 순서가 구분되지 않은 집합체

```xml
<!--
JSON Obejct => {"":""}
JSON Array => [,,,]
-->

{"example" : [
	{"Name":"John", "Age": 26},
	{"Name":"David", "Age": 24},
	{"Name":"Anna", "Age": 30}
]}
```



예제

```kotlin
// 1번 문제
fun readData(startIndex: Int, lastIndex: Int): JSONObject {
    val url = ${API URL}
    val connection = url.openConnection()
    val data = connection.getInputStream().readBytes().toString(charset("UTF-8"))
    return JSONObject(data)
}
val jsonObject = readData(startIndex, lastIndex)

/*
1번 문제
url connection에서 연결된 데이터를 String으로 받아온 후 JSONObejct로 받기
*/

// 2번 문제
val totalCount = jsonObject.getJSONObject("SearchPublicToiletPOIService").getInt("list_total_count")

/*
받아온 JSONObject에서 Key가 SearchPublicToiletPOIService의 value를 JSONObject 가져 온후
Key가 list_total_count 의 value를 가져 온다.
totalCount = 4938
*/

// 3번 문제
val rows = jsonObject.getJSONObject("SearchPublicToiletPOIService").getJSONArray("row")

/*
Key가 SearchPublicToiletPOIService의 value를 JSONObject로 받고
Key가 row 의 value를 JSONArray로 받아 rows에 초기화

*/
```

```xml
{
	"SearchPublicToiletPOIService": {
		"list_total_count": 4938,
		"RESULT": {
			"CODE": "INFO-000",
			"MESSAGE": "정상 처리되었습니다"
		},
		"row": [{
			"POI_ID": "102423",
			"FNAME": "우성스포츠센터",
			"ANAME": "민간개방화장실",
			"CNAME": "",
			"CENTER_X1": 192026.077328,
			"CENTER_Y1": 443662.903901,
			"X_WGS84": 126.90983237468618,
			"Y_WGS84": 37.49238614627172,
			"INSERTDATE": "20100712",
			"UPDATEDATE": "20100712"
		}, {
			"POI_ID": "102424",
			"FNAME": "프레곤빌딩",
			"ANAME": "민간개방화장실",
			"CNAME": "",
			"CENTER_X1": 191560.448911,
			"CENTER_Y1": 442968.699196,
			"X_WGS84": 126.90457509912622,
			"Y_WGS84": 37.48612718078578,
			"INSERTDATE": "20100712",
			"UPDATEDATE": "20100712"
		}, {
			"POI_ID": "102425",
			"FNAME": "하림빌딩",
			"ANAME": "민간개방화장실",
			"CNAME": "",
			"CENTER_X1": 201472.042745,
			"CENTER_Y1": 443869.796509,
			"X_WGS84": 127.01664600669827,
			"Y_WGS84": 37.49428349959237,
			"INSERTDATE": "20100712",
			"UPDATEDATE": "20100712"
		}]
    }
}
```





----

참고 사이트

JSON Array, Object 차이 Velog.io : https://velog.io/@dev_2dong/JSONObject%EC%99%80-JSONArray%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%99%80-%EC%A0%84%EB%B0%98%EC%A0%81%EC%9D%B8-JSON%ED%98%95%EC%8B%9D%EC%97%90-%EB%8C%80%ED%95%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0

----



## Android에서 Json

> 안드로이드에서 별도의 라이브러리 추가 없이 기본적으로 Json을 다룰 수 있는 json.org 패키지 제공

JSONObject, JsonArray 클래스는 전달받은 문자열을 파싱하여 객체화하고, key를 통해 value를 얻을 수 있는 메서드를 제공



## Gson 라이브러리

> google에서 제공하는 라이브러리로 객체 -> Json, 혹은 Json -> 객체로 변환하는 데 사용
>
> Java나 Kotlin의 기본적인 데이터 객체 뿐 아니라 사용자가 정의한 데이터 클래스도 간편하게 변환이 가능
>
> Json을 Serialize, Deserialize 를 돕는 라이브러리



**DataClass**

```kotlin
data class LoginForm(
	val id: String? = null,
  val password: String? = null
)
```

```kotlin
data class Profile(
	val name: String? = null
)
```

```kotlin
data class User(
	val loginForm: LoginForm? = null,
  val profile: Profile? = null
)
```





**라이브러리 추가**

```xml
<!-build.gradle (Module)->

dependencies {
	...
	implmentation 'com.google.code.gson:gson:x.x.x'
}
```



### Gson 생성

```kotlin
//1
val gson: Gson = GsonBuilder()
		.setPrettyPrinting()
		.serializeNulls()
		.excludeFieldsWithModifiers(Modifier.TRANSIENT)
    .setFieldNamingPolicy(FieldNamingPolicy.IDENTITY)
    .create()

//2
val gson = Gson()
```



#### setPrettyPrinting()

> Json 직렬화 할 때 보기 좋게 출력

Before

```kotlin
{"loginForm":{"id":"ghkdrnjsm","password":1234},"profile":{"name":"황인각"}}
```

After

```kotlin
{
  "loginForm": {
    "id": "ghkdrnjsm",
    "password": 1234
  },
  "profile": {
    "name": "황인각"
  }
}
```



#### serializeNulls()

> Null 값을 직렬화 / 역직렬화 (역직렬화 할 떄 값이 null 이면 기본값 사용)

Before

```kotlin
{
  "loginForm": {
    "id": "ghkdrnjsm",
  },
  "profile": {
    "name": "황인각"
  }
}
```

After

```kotlin
{
  "loginForm": {
    "id": "ghkdrnjsm",
    "password": null
  },
  "profile": {
    "name": "황인각"
  }
}
```





### Serialize

> 객체를 전송 가능한 형태로 만드는 것 (객체 -> JSON)

Object를 Json으로 바꾸는 방법

Gson to Json

```kotlin
fun serialize(){
  val user =User(
  	LoginForm("ghhkdrnjsm",null),
    Profile("황인각")
  )
  
  gson.toJson(user)
}
```

```xml
{
	"LoginForm":{
		"id":"ghkdrnjsm",
		"password":null
	},
	"profile":{
		"name":"황인각"
	}
}
```



### Deserialize

> 전송 가능한 형태를 객체로 만드는 것 (JSON -> 객체)

Json을 Object로 바꾸는 방법

Gson from Json

Json to Gson

```kotlin
fun deserialize(){
  val json = "{
      "LoginForm":{
        "id":"ghkdrnjsm",
        "password":null
      },
      "profile":{
        "name":"황인각"
      }
    }"
	gson.fromJson(json, User::class.java)
}
```



### Collection , Serialize - Deserialize

> Collection (Map, List, Array, Set등)을 직렬화, 역직렬화

```kotlin
fun collection(){
  // 객체를 JSON 으로 바꾸기
  val json = gson.toJson(listOf(
  	User(LoginForm("ID1","Password1"),Profile("Name1")),
    User(LoginForm("ID2","Password2"),Profile("Name2"))
  ))
  
  // JSON 을 객체로 바꾸기
  gson.fromJson<List<User>>(json, object: TypeToken<List<User>>(){}.type)
}
```







----

참고사이트

Kotlin in A..Z Gson 블로그 : https://rkdxowhd98.tistory.com/180

Gson 라이브러리 사용법 및 예제 : https://hianna.tistory.com/629