# Broadcast Receiver

> Broadcast를 수신할 수 있는 구성요소를 의미
>
> 앱이 특정 브로드캐스트를 수신하는 Broadcast Receiver를 등록하면,
>
> 시스템은 해당 리시버가 수신하는 브로드캐스타가 발생 할 때 마다 해당 앱에 브로드캐스트를 자동으로 라우팅 해준다.
>
> 또한 앱에서 브로드캐스트를 발생할 수도 있고, 다른 앱이 이를 수신받게 할 수도 있다



**Broadcast**

 안드로이드 시스템이나, 기타 앱에서 특정 이벤트가 발생할 때 시스템 전역에 이를 알리는 것

폰이 부팅됐을 때, 충전이 시작됐을 때, 전화가 왔을 때와 같은 다양한 이벤트가 발생할 수 있고, 이것이 발생할 때 브로드캐스트를 뿌리는 것이다.



**Broadcast 수신자**

브로드캐스트는 어떤 이벤트가 발생 할 때 시스템 전역에 이를 방송하는 것인데,

몇몇 앱들은 필요에 따라 관심 있는 브로드캐스트에 대해 수신하여 이벤트를 처리 할 수 있다.

예를 들어 충전이 시작 됐을 때 브로드캐스트가 발생하고, 만약 충전 시작 시 애니메이션을 재생하는 앱을 구현한다면 해당 브로드캐스트를 수신하여 동작을 구현 한다.



앱이 특정 브로드캐스트를 수신하는 Broadcast Receiver를 등록하면,

시스템은 해당 리시버가 수신하는 브로드캐스타가 발생 할 때 마다 해당 앱에 브로드캐스트를 자동으로 라우팅 해준다.



![img](https://media.vlpt.us/images/haero_kim/post/eb486844-bb5d-4458-8a15-17dfe3af789b/android_broadcastreceiver.png)



**Broadcast Receiver 활용**

시스템에서 지원하는 표준 브로드캐스트 목록 중 일부

- `ACTION_TIME_TICK`
- `ACTION_TIME_CHANGED`
- `ACTION_TIMEZONE_CHANGED`
- `ACTION_BOOT_COMPLETED`
- `ACTION_PACKAGE_ADDED`
- `ACTION_PACKAGE_CHANGED`
- `ACTION_PACKAGE_REMOVED`
- `ACTION_PACKAGE_RESTARTED`
- `ACTION_PACKAGE_DATA_CLEARED`
- `ACTION_PACKAGES_SUSPENDED`
- `ACTION_PACKAGES_UNSUSPENDED`
- `ACTION_UID_REMOVED`
- `ACTION_BATTERY_CHANGED`
- `ACTION_POWER_CONNECTED`
- `ACTION_POWER_DISCONNECTED`
- `ACTION_SHUTDOWN`



이러한 것들을 수신하면 스마트폰의 상태 변화, 각종 인터럽트 발생 등의 동작을 알아차릴 수 있고 앱에서 이들을 대응하는 동작을 구현 할 수 있다.



또한 앱에서 브로드캐스트를 발생할 수도 있고, 다른 앱이 이를 수신받게 할 수도 있다.

다방면으로 활용가능한 컴포넌트이다.



---

참고사이트

해로 velog : https://velog.io/@haero_kim/Android-%EC%A3%BC%EC%9A%94-4%EB%8C%80-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-Broadcast-Receiver