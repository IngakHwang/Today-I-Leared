# AlarmManager

> 정해진 시간에 알람을 받을 수 잇다.
>
> App이 실행 중이 아닐 때라도 정해진 시간에 이벤트를 받아 어떤 작업을 처리할 수 있다.



- 지정된 시간, 기간마다 App이 알람 이벤트를 받도록 설정 할 수 있다.
- 이벤트는 인텐트를 의미하며, 보통 BroadcastReceiver로 인텐트가 전달된다.
- AlarmManager가 이벤트를 보내기 때문에, 내 App이 실행 중이 아니더라도 알람을 받아 어떤 작업을 처리하도록 구현 할 수 있다.



## AlarmManager 권장사항

- 네트워크 작업보다 로컬 작업을 위해 AlarmManager를 사용하는 것이 좋다.
- 가능하다면 절전상태 일 때 알람을 받도록 설정하지 말것, 시스템 리소스가 빨리 소모 되어 사용자가 불편하다.
- 가능하다면 아람을 설정할 때 정확한 시간으로 설정하지 말 것
- 가능하다면 Real time 으로 설정하지 말고, Elapsed time(기기가 부팅된 후 경과한 시간)으로 시간을 설정 할것



## 1회성 알람 등록

> 알람을 특정 시간에 한 번만 받도록 설정할 수 있다.
>
> 알람을 받았을 때, 다시 알람을 등록한다면 반복적으로 알람을 받을 수 있다.



알람 이벤트는 브로드캐스트로 전달된다.

ex)

BroadcastReceiver를 만들고, 인텐트를 받으면 Notification을 띄우도록 구현

```kotlin
class AlarmReceiver : BroadcastReceiver() {

    companion object {
        const val TAG = "AlarmReceiver"
        const val NOTIFICATION_ID = 0
        const val PRIMARY_CHANNEL_ID = "primary_notification_channel"
    }

    lateinit var notificationManager: NotificationManager

    override fun onReceive(context: Context, intent: Intent) {
        Log.d(TAG, "Received intent : $intent")
        notificationManager = context.getSystemService(
                Context.NOTIFICATION_SERVICE) as NotificationManager

        createNotificationChannel()
        deliverNotification(context)
    }

    private fun deliverNotification(context: Context) {
        val contentIntent = Intent(context, MainActivity::class.java)
        val contentPendingIntent = PendingIntent.getActivity(
            context,
            NOTIFICATION_ID,
            contentIntent,
            PendingIntent.FLAG_UPDATE_CURRENT
        )
        val builder =
            NotificationCompat.Builder(context, PRIMARY_CHANNEL_ID)
                .setSmallIcon(R.drawable.ic_alarm)
                .setContentTitle("Alert")
                .setContentText("This is repeating alarm")
                .setContentIntent(contentPendingIntent)
                .setPriority(NotificationCompat.PRIORITY_HIGH)
                .setAutoCancel(true)
                .setDefaults(NotificationCompat.DEFAULT_ALL)

        notificationManager.notify(NOTIFICATION_ID, builder.build())
    }

    fun createNotificationChannel() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val notificationChannel = NotificationChannel(
                PRIMARY_CHANNEL_ID,
                "Stand up notification",
                NotificationManager.IMPORTANCE_HIGH
            )
            notificationChannel.enableLights(true)
            notificationChannel.lightColor = Color.RED
            notificationChannel.enableVibration(true)
            notificationChannel.description = "AlarmManager Tests"
            notificationManager.createNotificationChannel(
                    notificationChannel)
        }
    }
}
```

이 코드는 리시버에 대한 구현이다. 아직 AlarmManager 관련 코드를 구현하지 않았다.

여기서는 Notification을 등록하는 코드가 대부분이다.



Android Oreo 이상부터는 Notification을 띄울 때 먼저 channel을 등록해야 한다.

`createNotificationChannel()` 는 채널을 등록하는 코드이다.

채널이 등록되면 `deliverNotification()` 으로 Notification을 등록한다.



만든 리시버를 `AndroidManifest.xml` 에 등록해야 한다.

`exported` 속성을 `false` 로 설정하면 리시버는 내 앱으로부터 전달되는 인텐트만 받을 수 있다.

`true` 로 설정하면 다른 앱으로부터 전달되는 인텐트도 수신하게 된다.

```xml
<receiver android:name=".AlarmReceiver"
    android:exported="false">
```



### 알람 등록

알람을 등록하는 코드

ex) ToggleButton 눌렀을 때 알람을 등록 or 취소 하는 코드

```kotlin
val alarmManager = getSystemService(ALARM_SERVICE) as AlarmManager

val intent = Intent(this, AlarmReceiver::class.java)  // 1
val pendingIntent = PendingIntent.getBroadcast(     // 2
            this, AlarmReceiver.NOTIFICATION_ID, intent,
        PendingIntent.FLAG_UPDATE_CURRENT)

onetimeAlarmToggle.setOnCheckedChangeListener(OnCheckedChangeListener { _, isChecked ->
    val toastMessage = if (isChecked) {   // 3
        val triggerTime = (SystemClock.elapsedRealtime()  // 4
                + 60 * 1000)
        alarmManager.set(   // 5
                AlarmManager.ELAPSED_REALTIME_WAKEUP,
                triggerTime,
                pendingIntent
        )
        "Onetime Alarm On"
    } else {
        alarmManager.cancel(pendingIntent)    // 6
        "Onetime Alarm Off"
    }
    Log.d(TAG, toastMessage)
    Toast.makeText(this, toastMessage, Toast.LENGTH_SHORT).show()
})
```

각 주석에 대한 설명

1. 알람 조건이 충족되었을 때, 리시버로 전달될 인텐트를 설정
2. AlarmManager가 인텐트를 갖고 있다가 일정 시간이 흐른 뒤에 전달하기 때문에 PendingIntent로 만들어야 한다.
   - PendingIntent의 requestCode 인자가 전달된다
   - 여러 PendingIntent를 사용한다면 requestCode를 다르게 해줘야 한다.
   - 위 예제는 하나의 알람만 등록하기 때문에 `NOTIFICATION_ID` 를 사용한다.
   - flag는 아래에서 설명
3. ToggleButton이 눌리면 `isChecked=true`, 다시 눌리면 `isChecked=false` 가 된다.
4. elapsed time을 사용했고, 현재 시간부터 60초 뒤에 알람이 발생하도록 설정한다. (시간 단위 ms)
5. `set()` 을 이용하여 인자를 전달한다.
   - `ELAPSED_REALTIME_WAKEUP` 은 아래에서 설명
6. 알람을 취소 할 때는 등록한 PendingIntent를 인자로 전달한다.



`//2` 에서 PendingIntent를 만들 때 flag를 설정한다.

6개의 flag가 있고 의미는 다음과 같다.

- `FLAG_UPDATE_CURRENT` : 현재 PendingIntent를 유지하고, 대신 인텐트의 extra data는 새로 전달된 Intent로 교체
- `FLAG_CANCEL_CURRENT` : 현재 인텐트가 이미 등록되어있다면 삭제하고, 다시 등록한다.
- `FLAG_NO_CREATE` : 이미 등록된 인텐트가 있다면, 아무것도 하지 않는다.
- `FLAG_ONE_SHOT` : 한번 사용되면, 그 다음에 다시 사용되지 않는다.
- `FLAG_MUTABLE` : 생성된 PendingIntent가 변경 가능해야 함을 나타내는 플래그
- `FLAG_IMMUTABLE` : 생성된 PendingIntent가 변경할 수 없음을 나타내는 플래그



`//5` 에서 타입을 설정한다.

다음과 같은 타입들이 있다.

- `ELAPSED_REALTIME` : 기기가 부팅된 후 경과한 시간을 기준으로, 상대적인 시간을 사용하여 알람을 발생시킨다. 기기가 절전모드 (doze)에 있을 때는 알람을 발생시키지 않고 해제되면 발생시킨다.
- `ELAPSED_REALTIME_WAKEUP` : `ELAPSED_REALTIME` 과 동일하지만 절전모드일 때 알람을 발생시킨다.
- `RTC` : Real TIme Clock을 사용하여 알람을 발생, 절전모드 일 때는 알람을 발생시키지 않는다.
- `RTC_WAKEUP` : `RTC` 와 동일하지만 절전모드 일 때 알람을 발생시킨다.





----

참고사이트

코드차차 : https://codechacha.com/ko/android-alarmmanager/

tistory 블로그 : https://greedy0110.tistory.com/69

코드차차 노티피케이션 : https://codechacha.com/ko/notifications-in-android/