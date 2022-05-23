# Android Library



Android 라이브러리는 구조적으로 Android 앱 모듈과 동일하다.

Android 라이브러리에는 소스 코드, 리소스 파일, manifest 와 같이 앱을 빌드하는 데 필요한 모든 사항이 포함될 수 있다.

하지만 이 라이브러리는 기기에서 실행되는 APK로 컴파일되는 대신 Android 앱 모듈의 종속성으로 사용할 수 있는 AAR에 컴파일된다.

JAR 파일과 달리 AAR 파일에는 Android 리소스 및 manifest 파일이 포함될 수 있다.

그래서 클래스 및 메서드 외에 레이아웃, 드로어블과 같은 공유 리소스를 번들로 구성할 수 있다.



라이브러리 모듈은 다음과 같은 상황에 유용

- Activity, Service, UI Layout 등 일부 구성요소를 동일하게 사용하는 여러 앱을 빌드하는 경우
- 여러 APK 변형(예 : 유료, 무료 버전)에 포함되는 앱을 빌드하며 두 버전에서 모두 동일한 핵심 구성요소가 필요한 경우



두 가지 경우 모두 재사용하려는 파일을 라이브러리 모듈로 이동한 후 각 앱 모듈의 종속성으로 라이브러리를 추가하면 된다.



## 앱 모듈을 라이브러리 모듈로 변환

재사용하려는 코드가 모두 포함된 기존 앱 모듈이 있는 경우 다음과 같이 이 모듈을 라이브러리 모듈로 변환할 수 있다.

1. build.gradle 파일(Module Level) 열기

2. applicationID 를 지정하는 줄 삭제, Android App Module 에서만 지정 할 수 있음

3. 파일 상단에 다음 표시를 변경

   이전 :  `apply plugin: 'com.android.application'`

   변경 :  `apply plugin: 'com.android.library'`

4. 파일 저장하고 File > Sync Project with Gradle Files 클릭



모듈 전체 구조는 동일하게 유지되지만, 이제 Android Library로 작동하며 빌드에서 이제 APK 대신 AAR 파일이 생성된다.

AAR 파일을 빌드하려면 Project 창에서 라이브러리 모듈을 선택한 다음 Build > Build APK 를 클릭한다.



## 라이브러리를 종속성으로 추가

Android 라이브러리의 코드를 다른 앱 모듈에 사용

1. 다음 두 가지 방법 중 하나로 라이브러리를 프로젝트에 추가

   같은 프로젝트 내에서 라이브러리 모듈을 생성한 경우에는 라이브러리 모듈이 이미 있으므로 이 단계를 건너뛸 수 있음

   - 컴파일된 AAR(or JAR) 파일을 추가 (라이브러리가 이미 빌드되어야 함)

     1. File > New > New Module 클릭
     2. Import .JAR/.AAR Package 클릭 - Next 클릭
     3. AAR or JAR 파일 위치를 입력 후 Finish 클릭

   - 라이브러리 모듈을 프로젝트로 가져오기 (라이브러리 소스가 프로젝트에 포함)

     1. File > New > Import Module 클릭
     2. 라이브러리 모듈 디렉터리의 위치를 입력 후 Finish 클릭

     

   라이브러리 모듈이 프로젝트에 복사되고, 라이브러리 코드를 실제로 수정할 수 있다.

   단일 버전의 라이브러리 코드를 유지하려는 겨우에는 이 작업을 원치 않을 수 있다.

   이때는 위에 설명된 대로 컴파일된 AAR 파일을 추가해야 한다. 

   

2. setting.gradle 파일의 맨 위에 라이브러리가 표시되는지 확인

   Include ':app', ':my-library-module'

   

3. build.gradle (app module) 파일을 열고 dependencies 블록에 새 줄 추가

   ```xml
   dependencies{
   	implementation project(":my-library-module")
   }
   ```

   

4. Sync Project with Gradle Files 클릭

   위의 예시에서 implementation 구성은 my-library-module 이라는 라이브러리를 전체 앱 모듈의 빌드 종속성으로 추가

   대신 특정 빌드 변형 전용 라이브러리를 사용하는 경우 implementation 대신 buildVariantNameImplementation 사용



## 라이브러리 개발 고려 사항

라이브러리 모듈과 종속 앱을 개발할 때에는 다음 동작 및 제한 사항에 유의해야 한다.

라이브러리 모듈에 대한 참조를 Android 앱 모듈에 추가한 후에는 모듈의 상대적인 우선순위를 설정할 수 있다.

빌드 시에는 가장 낮은 우선순위에서 가장 높은 우선순위 순으로 라이브러리가 한 번에 하나씩 앱과 병합된다.



- 하드코딩 금지

  

- 변수 이름 잘 짓기

  

- 리소스 병합출동

  빌드 도구는 라이브러리 모듈의 리소스를 종속 앱 모듈의 리소스와 병합한다.

  지정된 리소스 ID가 두 모듈에서 모두 정의된 경우에는 앱의 리소스가 사용된다.

  여러 AAR 라이브러리 간에 충돌이 발생한 경우 종속성 목록에 가장 먼저 (dependencies 블록의 윗 부분) 나열된 라이브러리의 리소스가 사용된다.

  일반 리소스 ID와 관련된 리소스 충돌을 방지하려면 해당 모듈을 고유하거나 모든 프로젝트 모듈 전체에 걸쳐 고유한 접두사 또는 기타 일관된 이름 지정 체계를 사용하는 것이 좋다.

  

- 다중 모듈 빌드에서는 JAR 종속성이 전이 종속성으로 처리

  AAR을 출력하는 라이브러리 프로젝트에 JAR 종속성을 추가하면 JAR이 라이브러리 모듈에서 처리되어 AAR과 함께 패키징 된다.

  하지만 프로젝트에 앱 모듈에서 사용하는 라이브러리 모듈이 포함되어 있으면 앱 모듈은 라이브러리의 로컬 JAR 종속성을 전이 종속성으로 처리한다.

  이 경우 로컬 JAR은 라이브러리 모듈이 아닌 로컬 JAR을 사용하는 앱 모듈에서 처리 한다.

  이를 통해 라이브러리의 코드를 변경하면 발생하는 증분 빌드 속도가 빨라진다.

  로컬 JAR 종속성에 의한 자바 리소스 충돌은 라이브러리를 사용하는 앱 모듈에서 해결되어야 한다.

  

- 라이브러리 모듈은 외부 JAR 라이브러리에 종속될 수 있음

  외부 라이브러리에 종속된 라이브러리 모듈을 개발 할 수 있다. (예 : Maps 외부 라이브러리)

  이 경우 종속 앱은 외부 라이브러리를 포함하는 타겟 (예 : Google API 부가기능)을 대상으로 빌드해야 한다.

  라이브러리 모듈과 종속 앱 모두 manifest 파일의 <uses-library> 요소에서 외부 라이브러리를 선언해야 한다.

  

- 앱 모듈의 minSdkVersion은 라이브러리에서 정의된 버전 보다 크거나 같아야 함

  라이브러리는 종속 앱 모듈의 일부로 컴파일되므로 라이브러리 모듈에서 사용되는 API가 앱 모듈이 지원하는 플랫폼 버전과 호환되어야 한다.

  

- 각 라이브러리 모듈은 자체 R 클래스를 생성

  종속 앱 모듈을 빌드하면 라이브러리 모듈이 AAR 파일로 컴파일된 후 앱 모듈에 추가된다.

  따라서 각 라이브러리에는 라이브러리의 패키지 이름을 따라 이름이 지정된 자체 R 클래스가 있다.

  기본 모듈과 라이브러리 모듈에서 생성되는 R 클래스는 기본 모듈의 패키지와 라이브러리의 패키지를 포함하여 필요한 모든 패키지에서 생성



## AAR 파일 분석

AAR 파일 확장자는 .aar

Maven 아티팩트 유형도 aar

파일 자체는 다음 필수 항목을 포함하는 압축파일이다.

- /AndroidManifest.xml
- /classes.jar
- /res/
- /R.txt
- /public.txt



또한 AAR 파일에는 다음과 같은 선택적인 항목도 하나 이상 포함될 수 있다.

- /assets/
- /libs/names.jar
- /jni/abi_name/name.so
- /proguard.txt
- /lint.jar
- /api.jar



## 라이브러리 문서 작성

문서 작성 시에 고려해야 할 것

- 라이브러리의 목적성
- 라이브러리 사용방법
- 라이브러리 요청/응답 데이터 (사용자들이 무슨 데이터인지 알기 위해서)
- 문서 작성 시에도 로직을 세워야 한다.



## 패키지



### 패키지 이름(Package Name)

- 애플리케이션을 고분하는 고유한 값
- 내가 만든 앱이 디바이스에 설치되었을 때 다른 앱들과 구분하는 역할을 하므로 유일무이해야 한다.



### 패키지 명명 방법

Ex)

| 명명방법                           | 예시                             |
| ---------------------------------- | -------------------------------- |
| com.회사이름.프로그램이름          | com.autocrypt.program            |
| com.회사이름.플랫폼.프로그램이름   | com.autocrypt.platform.program   |
| kr.co.회사이름.프로그램이름        | kr.co.autocrypt.program          |
| kr.co.회사이름.플랫폼.프로그램이름 | kr.co.autocrypt.platform.program |



**명명규칙**

- 회사 이름이나 도메인 등은 유니크하기 때문에 사이트명으로 많이 구분
- 웹사이트 주소를 반대로 기재한 모양으로 패키지 이름을 부여
- 명칭 소문자 사용
- 패키지명에 대문자는 사용하지 않는 것이 좋다.



## Android Library AAR 파일 만들기

1. Create New Module - Android Library 에서 Module name 기재 후 Finish

   <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2ieomlifkj214s0tatb2.jpg" alt="image-20220523171327319" style="zoom:50%;" />

2. `build.gradle (Module : 라이브러리)` 에서 plugins 내 id `com.android.application` 에서 `com.android.library` 로 변경

   defaultConfig에서 applicationId 삭제 후 Sync Now

   <img src="https://blog.kakaocdn.net/dn/mnYWj/btq6PDZzGQr/bbHpbzmQiv6d0Nb0g5DTbK/img.png" alt="img" style="zoom:50%;" />

3. Android Studio 우측 상단에 Gradle - 코끼리모양 (Execute Gradle Task) 클릭

   <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2ietapnuij21ys0u0jxf.jpg" alt="image-20220523171758902" style="zoom:50%;" />

4. gradle assembleRelease 입력 후 Enter (오타나면 안됨)

   <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2ieuouihbj21140nawf3.jpg" alt="image-20220523171846580" style="zoom:50%;" />

5. `프로젝트 - 라이브러리 - build - outputs - aar` 경로에 파일 확인





----

참고 사이트

공홈 : https://developer.android.com/studio/projects/android-library?hl=ko

티스토리 : https://keinetwork.tistory.com/138 , https://jroomstudio.tistory.com/78