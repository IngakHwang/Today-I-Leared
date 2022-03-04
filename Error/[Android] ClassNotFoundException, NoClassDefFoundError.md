# [Android] ClassNotFoundException, NoClassDefFoundError

> 컴파일 시 클래스 파일 빌드에 대한 오류
>
> 클래스 파일 간에 비슷한 이름이 있는지 확인 하자



ClassNotFoundException : ClassPath에 로드하고자 하는 Class가 발견되지 않을 때 발생

NoClassDefFoundError : 컴파일 시점에 존재했던 클래스가 런타임에 존재하지 않으면 발생

![img](https://blog.kakaocdn.net/dn/cJopex/btqYQwVz3q1/8fVBIMQRNKeEkaSPaTeB9k/img.png)



## 에러 발생

3/3, 3/4

안드로이드 자바 프로젝트를 코틀린화 하는 작업에서 위와 같은 에러들이 발생하여 정상적인 실행이 되지 않았다.



### 조치 했었던 것

1. Build - Clean Project, Rebuild Project 클릭 

   효과 없었음

2. 에뮬레이터 - Cold Boot Now 실행

   효과 없었음

3. File - invalidate Caches... 클릭

   효과 없었음



### 원인 추측

원인이 되는 recyclerView의 사용되는 어댑터 클래스는

이전에 작성했던 자바 클래스(mainAdapter.java)와 

현재 작성 중인 코틀린 클래스(MainAdapter.kt)의 이름이 앞의 대문자 유무만 빼면 비슷하여 컴파일이 잘못 되었을 것이라는 추측



### 해결 조치

이름이 비슷한 클래스들은 따로 디렉토리를 만들어 분리 후 코드 실행 결과 정상적으로 실행



---

참고 사이트

티스토리 블로그 : https://yangbox.tistory.com/117