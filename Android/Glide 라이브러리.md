# Glide 라이브러리



Glide 라이브러리가 정확하게 무엇이고 어떻게 사용하는 건지 알기 위함



## Glide란?



사전적의미 : 미끄러지는 듯한 움직임, 미끄러짐

Goolge에서 밀고 있는 안드로이드 이미지 로딩 라이브러리이다.

기본적으로 사진 로딩 기능과 심화적으로 동영상, GIF 로딩 기능까지 지원한다.



## 사용법



### Gradle 추가

```xml
implementation 'com.github.bumptech.glide:glide:4.9.0'
annotationProcessor 'com.github.bumptech.glide:compiler:4.9.0'
```

버전은 매주기 마다 업데이트



### 대표적인 함수

#### override()

> 이미지를 지정한 크기만큼 불러온다. 로딩 속도를 빠르게 하고 메모리를 절약하고 싶을 떄 유용

```kotlin
Glide.with(this)
			.load("이미지 url")
			.override(이미지 사이즈)	// ex) override(600,200)
			.into(binging.imgview)
```



#### placeholder()

> 이미지가 로딩하는 동안 보여질 이미지를 정한다.

```kotlin
Glide.with(this)
			.load("이미지 url")
			.placehodler(로딩 이미지)	//	ex) placeholder(R.drawable.loading)
			.into(binding.imgview)
```



#### error()

> 이미지를 불러오는 데 실패 했을 때 보여질 이미지를 정한다.

```kotlin
Glide.with(this)
			.load("이미지 url")
			.error(실패 이미지)	//	ex) error(R.drawable.error)
			.into(binding.imgview)
```



#### asGif()

> GIF 이미지를 로딩할 때 호출하는 함수

```kotlin
Glide.with(this)
			.load("이미지 url")
			.into(binding.imgview)
			.asGif()
```



---

참고 사이트

velog : https://velog.io/@rjsdnqkr1/Glide-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-yuk1fmwzo1