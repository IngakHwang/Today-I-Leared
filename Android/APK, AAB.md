# APK, AAB



![AAB(Android App Bundle)](https://yozm.wishket.com/media/news/912/image002.png)

APK (Android Package)는 이미 완성된 안드로이드 앱 파일

AAB (Android App Bundle)는 APK를 완성해주는 요소를 담은 패키지



ex)

어떤 앱을 설치 할 때, 

한국어 기반의 갤럭시 S21과 독일어 기반의 갤럭시 S10 두 경우 모두 ㄱ똑같은 APK 파일이 설치되는 것이 지금까지의 방법이었지만,

AAB 형식으로 배포될 경우 사용자 기기에 필요한 파일만으로 구성된, 군더더기가 제거된 APK 파일을 설치 할 수 있는 것이다.



기존처럼 모든 기기에 대응할 수 있는 하나의 APK를 전달하는 것이 아니라, 개발자가 스토어에 AAB 패키지를 올려 놓으면, 스토어가 사용자 기기에 어떤 내용이 필요한지 확인 후 그에 맞춘 APK 파일을 만들어 배포하는 식이다.



## AAB 장점

> 앱의 크기가 줄어든다는 것이 큰 장점

앱의 크기가 줄어든다는 것이 가장 큰 장점이다.

이론적으로만 생각하면 개발자가 각 사용자들에게 맞는 APK 파일 버전을 여러 개 만들어 배포하는 것이 가장 좋지만, 그렇게 하기엔 안드로이드 기기의 종류와 언어가 너무 많다.

그리고 업데이트가 있을 때마다 그 모든 버전을 최신화 하는 것은 물리적으로 불가능하다.



그러니 개발자는 앱을 구성하는 요소들을 레고 블록처럼 잘 나누어놓은 다음, 그 블록들을 구글 플레이 스토어에 올려 놓는 것이다.

그러면 스토어 측에서 각 사용자에게 맞는 레고 블록만 모아서 조립 후 배포하는 식이다.

이러면 개발자입장에서는 일일이 맞춤형으로 제작하지 않아도 되고, 사용자 입장에서 필요 없는 기능은 받지 않아도 된다.

평균적으로 AAB 형태로 배포하면 앱 크기가 15% 줄어든다고 한다.



## AAB 단점

> 보안에 대한 우려가 있다.

보안에 대한 우려가 있다.

모든 안드로이드 앱에는 개발자의 서명 파일이 들어간다.

서명을 하여 '이 앱을 만든 사람은 나' 라는 증거를 남기는 것이다.

서명 파일은 개발자가 APK 파일에 직접 첨부한다.

따라서 누군가가 앱을 멋대로 변형해 배포하려고 해도, 원래 개발자의 서명이 없기 때문에 공식 앱이 아님을 확인 할 수 있다.



하지만 AAB파일은 위에서 짚었듯이 완성된 앱이 아니라 완성품으로 태어날 수 있는 레고 블록의 모음이다.

그리고 사용자를 위해 레고 블록을 끼워 맞추는 것은 개발자가 아니라 구글 플레이다.

따라서 서명은 개발자가 아닌 구글 플레이가 대신하게 된다.



다른 이가 나 대신 서명하는 것은 찝찝함이 있을 수 밖에 없다.





### AAB 배포 파일 만들기

1. Android Studio 메뉴 Build - Generate Signed Bundle / APK... 메뉴 선택

![img](https://blog.kakaocdn.net/dn/2wLxl/btqXb2HhssD/EsNI9IIDHyOHKaawEPr7k0/img.png)

2. 배포.방식은 아래와 같이 2가지 방식이 존재
   - Android App Bundle 
   - APK

![img](https://blog.kakaocdn.net/dn/wFkhr/btqXb19q2Xd/YfYmdlX1wXcbyGRVeh4rUk/img.png)

3. Next 버튼 누른 후, 앱의 Key 정보를 관리

![img](https://blog.kakaocdn.net/dn/mRIxs/btqXobwzVE0/rDXsNJaMfoXVveVvK345g0/img.png)

4. 생성될 배포파일이 debug인지, release인지 선택

![img](https://blog.kakaocdn.net/dn/JAcSz/btqXobXDKAk/k2NaoQJmStJKqkj1Y8cMj1/img.png)



## Tip

**관용적으로 빌드파일 이름 짓는 법**

앱이름 + 버전 + realease or debug

Ex -> `QRCodeReaderApp_2.4.1_realease`



**빌드 전 clean project 하기**

안 할 시 빌드후에 안될 수도 있음





----

참고 사이트

위시켓 : https://yozm.wishket.com/magazine/detail/912/

티스토리 : https://sgpassion.tistory.com/564