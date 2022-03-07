# SimpleDateFormat

> 날짜, 시간을 원하는 포맷으로 출력하고 싶을 때 사용하는 클래스



먼저 포맷을 생성하는 데 쓰이는 문자에 따라서 출력이 다르게 나온다.

Date or Time Componet를 참고해서 원하는 Letter를 사용하면 된다.

![image-20220307165301309](https://tva1.sinaimg.cn/large/e6c9d24egy1h01ddo29flj20x40u00ve.jpg)



## 사용법

```kotlin
val sdf = SimpleDateFormat("HH:mm:ss")
val time = sdf.format(시간값)
```

- SimpleDateFormat("원하는 형식") - 위 표에 있는 Letter를 참고하여 원하는 포맷을 작성한다.
- SimpleDateFormat.format(시간 값) - format함수를 사용하여 시간 값을 원하는 포맷으로 변경한다.



ex) 현재 날짜 출력

```kotlin
val now = System.currentTimeMillis()
val simpleDateFormat = SimpleDateFormat("yyyy.MM.dd", Locale.KOREAN).format(now)

print(simpleDateFormat)				// 2022.03.07 
```



ex) 현재 시간 출력

```kotlin
val now = System.currentTimeMillis()
val simpleDateFormat = SimpleDateFormat("HH:mm:ss", Locale.KOREAN).format(now)

print(simpleDateFormat)				// 16:57:21
```



Locale : 현장, 무대

Locale은 표현하고자 하는 지역을 나타낸다고 생각하자



----

참고사이트

티스토리 블로그 : https://ddusi-dod.tistory.com/32