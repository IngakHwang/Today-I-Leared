# Android Gradle

> 빌드 및 라이브러리 관련 설정 관리

<img src="https://blog.kakaocdn.net/dn/er6FA8/btq9fxiccie/m3oGGpM8FcWB0Ym4m2fjnK/img.png" alt="img" style="zoom:50%;" />



## Gradle 이란?

CI/CD를 위한 아래 Task 들을 자동화 시켜주는 Build Tool

- Compile
- Test
- Packaging
- Deploy & Run



Compile은 Kotlin, Java 파일을 바이트 코드로 변환해주는 작업

Test 는 앱이 제대로 동작할지에 대한 Test (유닛테스트, UI 테스트 등) 지원

Packaging은 코드를 패키징하여 aab 파일, apk 파일을 만들어 주는 것

Deploy & Run 은 코드를 앱으로 패키징해서 실제 기기에 넣어서 실행할 수 있도록 만들어주는 것



## build.gradle (Project : 프로젝트명)

> 프로젝트 수준의 gradle 설정 파일

<img src="https://blog.kakaocdn.net/dn/bYj8Re/btq9fXufYFm/gSc79F1fpLpgocv0um9Xr1/img.png" alt="img" style="zoom:50%;" />



### buildscript

- `repositories` : 외부 저장소 설정, `google()` 이 기본으로 설정
- `dependencies` (의존성 라이브러리) : gradle 플러그인 버전 설정



### allprojects

- `respositories` : 위의 buildscript > respositories와 동일한 외부저장소가 설정



### task

> 프로젝트 전체적으로 공통으로 사용할 작업을 정의

- `clean(type : Delete)` : 기본으로 추가된 공통작업으로, 빌드시 생성되는 build 디렉터리들을 삭제 



## build.gradle (Module : 프로젝트명.app)

> 모듈 수준의 gradle 설정 파일
>
> 모듈 정류는 app모듈, 웨어러블 모듈, 안드로이드TV 모듈 등이 있다.
>
> 보통 기본으로 app 모듈 수준의 빌드설정 / 라이브러리 정보가 저장



<img src="https://blog.kakaocdn.net/dn/REkTL/btq9fkRcljp/vVEgkAksN5xKwK7Ur51DD1/img.png" alt="img" style="zoom:50%;" />

### plugins

> 안드로이드 개발을 위한 플러그인 설정 영역
>
> `com.android.application` 기본으로 설정



### android

> 컴파일/빌드, 버전정보, 난독여부 등을 설정

- `compileSdkVersion` : 컴파일 SDK 버전 설정
- `defaultConfig` : 각종 버전 정보 설정
- `buildTypes` : 빌드 설정
- `compileOptions` : 컴파일 설정



### dependencies

> 외부 라이브러리 설정



## gradle-wrapper.properties (Grandle Version)

> Gradle 자체와 관련된 설정 파일

<img src="https://blog.kakaocdn.net/dn/bgWwBx/btq9e6ySXz3/QXlpdF4ua9FcF0b8C15Ty1/img.png" alt="img" style="zoom:50%;" />



## proguard-rules.pro (ProGuard Rules for 프로젝트명.app)

> 코드 난독화 도구 (ProGuard) 설정파일
>
> 코드 난독화 시 추가할 규칙이 있다면 이 파일에 기술

<img src="https://blog.kakaocdn.net/dn/byTYKL/btq9eMgmpgG/H4kqJioWXDhgw5VyAVpLf0/img.png" alt="img" style="zoom:50%;" />



## gradle.properties (Project Properties)

> 프로젝트 수준의 gradle 환경 설정 파일

<img src="https://blog.kakaocdn.net/dn/dNED4G/btq9eMUYWEW/jnBAxmxEEym5um5xttuTn1/img.png" alt="img" style="zoom:50%;" />



## settings.gradle (Project Settings)

> 프로젝트에 포함된 모듈을 등록/관리하는 파일
>
> Phone&Tablet 으로 프로젝트 생성한 경우, 아래처럼 'app' 모듈만 기본으로 등록
>
> 웨어러블 모듈, 안드로이드 TV모듈을 추가하면 여기에 등록된다.

<img src="https://blog.kakaocdn.net/dn/wyfJx/btq9gYfljc4/AoszpcF3MJHp8sUzwAck5k/img.png" alt="img" style="zoom:50%;" />



## local.properties (SDK Location)

> 안드로이드 SDK 경로를 관리하는 파일
>
> 열어보면 그냥 안드로이드 SDK가 설치된 경로만 적혀있음

<img src="https://blog.kakaocdn.net/dn/r0tXG/btq9e7kk4Yg/bHMeJ6wReE12hYOjlTQu00/img.png" alt="img" style="zoom:50%;" />



----

참고사이트

티스토리 : https://uroa.tistory.com/64 , https://curryyou.tistory.com/368

공홈 : https://developer.android.com/studio/build?hl=ko