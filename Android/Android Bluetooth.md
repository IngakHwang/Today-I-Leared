# Android Bluetooth

> 스마트폰, 노트북, 이어폰, 헤드폰 등의 전자기기를 서로 연결하여 종보를 교환하는 근거리 무선 기술 (10M 이내)의 표준



## 원리

블루투스의 원리 = 무선전파의 원리

안테나를 통해 보내고자 하는 데이터를 전자기파로 변조하여 보내고

안테나가 수신하여 전자기파를 데이터로 복조 시키는 것

(복조 : 데이터 통신에서 수신된 신호를 원래의 신호로 복구하는 조작)



## Bluetooth와 BLE 차이

BLE : Bluetooth Low Energy



BLE는 개인용 무선 네트워크 (Wireless Personal Area Network, WPAN) 기술의 일종

기존 블루투스 (Bluetooth Classic, BR/EDR)과는 사실상 별개의 기술



차이점은 블루투스(BR/EDR + HS) 기술이 데이터 전송 속도 (data rate)에 초점을 맞추고 있는 반면,

BLE은 전력 소모를 줄이는 것에 초점을 맞추며 헬스케어, 피트니스, 보안 시스템 등의 특정 응용분야에 적합한 기술로 대두

​	저전력, 저비용, 단순성, 저속도, 소형 지향

​	BLE 로만 동작하는 싱글 모드 (Bluetooth SMART)

​	BLE + 블루투스 모두 지원하는 듀얼 모드 (Bluetooth SMART Ready)

![img](http://www.ktword.co.kr/img_data/5403_1.JPG)

![img](https://iotlab.tertiumcloud.com/wp-content/uploads/2020/08/Classic-Bluetooth-vs-BLE.png)

---

BR : Basic Rate

EDR : Enhanced Date Rate

HS : High Speed

---

### BLE 특징

- 연결 중 수면 시간 (sleep time)

- 작은 크기의 데이터 전송 단위

- 매우 낮은 Duty Cycle

- 주파수 대역 : 기존 블루투스와 동일 (2.4GHz)

- 사용 채널 : 2 MHz 폭씩 40개 채널

- 전력 소모 : 15 mA 이내

- 전송 속도 : 1Mbps

- 최대 전송 전력 : 10 mW

- 통신 방향 : 양방향 모두 지원 가능 하지만, 대개 단방향성만 지원

  



## Bluetooth Permission

Manifest.xml

```xml
...
		<uses-permission android:name="android.permission.BLUETOOTH"/>
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
    <uses-permission android:name="android.permission.BLUETOOTH_SCAN"/>
    <uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE"/>

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

    <uses-feature android:name="android.hardware.bluetooth"/>
    <uses-feature android:name="android.hardware.bluetooth_le"/>
...
```



**Android.permission**

---

ACCESS_FINE_LOCATION : 앱이 정확한 위치에 액세스할 수 있도록 허용

ACCESS_COARSE_LOCATION : 앱이 대략적인 위치에 액세스할 수 있도록 허용

BLUETOOTH : 앱이 페어링된 블루투스 기기에 연결할 수 있도록 허용

BLUETOOTH_ADMIN : 앱이 블루투스 기기를 검색하고 페어링 할 수 있도록 허용

BLUETOOTH_ADVERTISE : 주변 블루투스 기기에게 광고 ("나 블루투스 기기 입니다~~")

BLUETOOTH_CONNECT : 페어링된 블루투스 장치에 연결하는데 필요

BLUETOOTH_PRIVILEGED : 앱이 사용자 상호 작용 없이 블루투스 장치를 페어링하고 전화번호부 접근, 또는 메시지 접근을 허용하거나 허용하지 않도록

BLUETOOTH_SCAN : 근처에 있는 블루투스 장치를 검새갛고 페어링하는 데 필요

참고 사이트 : 공홈 - https://developer.android.com/reference/android/Manifest.permission

----



**Android.hardware**

----

bluetooth : 앱이 일반적으로 다른 블루투스 사용 기기와 통신하기 위해 기기의 블루투스 기능을 사용

bluetooth_le : 앱이 기기의 저전력 블루투스 무선 기능을 사용

location : 앱이 일반적으로 다른 소비자용 적외선 기기와 통신하기 위해 기기의 적외선 기능을 사용

location.gps : 앱이 기기에서 GPS 수신기가 제공하는 정확한 위치 좌표를 사용

location.network : 앱이 기기에 지원되는 네트워크 기반 위치정보 시스템이 제공하는 대략적인 위치 좌표를 사용

참고 사이트 : 공홈 - https://developer.android.com/guide/topics/manifest/uses-feature-element#features-reference

----



Permission check method

```kotlin
fun checkPermissions(){
  if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.S){
            requestPermissions(arrayOf(
                Manifest.permission.BLUETOOTH,
                Manifest.permission.BLUETOOTH_SCAN,
                Manifest.permission.BLUETOOTH_ADVERTISE,
                Manifest.permission.BLUETOOTH_CONNECT
            ),1)
        }else{
            requestPermissions(arrayOf(
               Manifest.permission.BLUETOOTH
            ),1)
        }
    }
}
```





## Bluetooth On/Off



bluetooth On/Off method

```kotlin
var bluetoothAdpater : BluetoothAdpater? = null
bluetoothAdapter = BluetoothAdpater.getDefaultAdapter()

fun bluetoothOnOff(){
  if(bluetoothAdapter == null){
    Log.d("bluetoothAdapter", "Device does not support Bluetooth")
  }
  else {
    if(bluetoothAdpater?.isEnabled == false){			//블루투스 기능 꺼져 있으면 블루투스 활성화
      val enableBtIntent = Intent(BluetoothAdpater.ACTION_REQUEST_ENABLE)
      startActivityForResult(enalbeBtIntent, REQUEST_ENABLE_BT)
    } else {
      bluetoothAdpater?.disable()									//블루투스 기능 켜져 있으면 블루투스 비활성화
    }
  }
}
```



## BluetoothBLE Scan



### Bluetooth Scan을 위한 Permission 확인

```kotlin
val PERMISSIONS = arrayOf{
  Manifest.permission.ACCESS_FINE_LOCATION
}

fun hasPermissions(context : Context?, permissions : Array<String>) : Boolean {
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M && context != null && permissions != null) {
            for (permission in permissions) {
                if (ActivityCompat.checkSelfPermission(context, permission)
                    != PackageManager.PERMISSION_GRANTED) {
                    return false
                }
            }
        }
  return true
}
```

안드로이드 버전이 M 이상이고 context도 있고, permissions도 있으면 false turn 아니면 true return



### Bluetooth Device Scan

```kotlin
var scanning : Boolean = false									// scan 중인지 상태 확인
var devicesArr = ArrayList<BluetoothDevice>()		// scan 한 Device를 담는 배열
val SCAN_PERIOD = 1000													// scan 시간 
val handler = Handler()

val mLeScanCallback = Object : ScanCallback(){
    override fun onScanFailed(errorCode: Int) {
        super.onScanFailed(errorCode) 
        Log.d("scanCallback", "BLE Scan Failed : " + errorCode)
    }
    override fun onBatchScanResults(results: MutableList<ScanResult > ?) {
        super.onBatchScanResults(results) 
        results?.let {
            for(result in it) {
                if(!devicesArr.contains(result.device) && result.device.name!=null) devicesArr.add(result.device)
            }
        }
    }
    override fun onScanResult(callbackType: Int, result: ScanResult?) {
        super.onScanResult(callbackType, result) 
        result?.let {
            if(!devicesArr.contains(it.device) && it.device.name!=null) devicesArr.add(it.device) 
            recyclerViewAdapter.notifyDataSetChanged()
        }
    }
}

```

ScanCallbacK

onScanFailed() : scan에 실패했을 때 실행

onBatchScanResults() : Batch Scan Result가 전달될 떄 콜백

onScanResult() : BLE advertisement가 발견되었을 때 실행



**scanDevcie**

매개변수 state가 true 면 handler 이용해 Bluetooth Scan을 SCAN_PERIOD 동안 실행하고

false 이면, Scanning 멈춘다.

```kotlin
private fun scanDevice(state : Boolean) = if(state){
        handler.postDelayed({
            scanning = false
            bluetoothAdapter?.bluetoothLeScanner?.stopScan(mLeScanCallback)
        }, SCAN_PERIOD.toLong())
        scanning = true
        devicesArr.clear()
        bluetoothAdapter?.bluetoothLeScanner?.startScan(mLeScanCallback)
    } else{
        scanning = false
        bluetoothAdapter?.bluetoothLeScanner?.stopScan(mLeScanCallback)
    }
```



scanBtn 클릭 시 Permission 검사 후 필요 Permission 요청한 후, ScanDevice(true)를 통해 Bluetooth Device Scan

```kotlin
binding.scanBtn.setOnClickListener{
  if(!hasPermissions(this, PERMISSIONS)){
    requestPermissions(PERMISSIONS, 2)
  }
  scanDevice(true)
}
```



## Bluetooth Scan

기기 검색을 시작하려면 `startDiscovery()` 를 호출하면 된다. (BluetoothAdapter 객체)

이 프로세스는 비동기식이고 검색이 성공적으로 시작되었는지 나타내는부울 값을 반환한다.

일반적으로 검색 프로세스는 12초 정도의 조회 스캔과, 검색된 각 긱기의 페이지 스캔을 통해 블루투스 이름을 가져오는 과정이 포함된다.



앱이 검색된 각 기기에 대한 정보를 수신하려면 `ACTION_FOUND` 인텐트에 대한 BroadcastReceiver를 등록해야 한다.

시스템이 각 기기에 대하여 이 인텐트를 브로드캐스트한다.

인텐트에는 엑스트라 필드 `EXTRA_DEVICE` , `EXTRA_CLASS` 가 포함되고, 이 엑스트라 필드에는 각각 `BluetoothDevice` , `BluetoothClass` 가 포함된다.

**기기 검색 시 브로드캐스트 처리등록**

```kotlin
binding.bluetoothScanBtn.setOnClickListener{
  bluetoothAdapter.startDiscovery()						//기기 탐색
  
  val filter = IntentFilter(BluetoothDevice.ACTION_FOUND)
  registerReceiver(receiver, filter)					//탐색 결과 리시버
  
  recyclerViewAdapter.notifyDataSetChanged()	//recyclerView itemUpdate
}

val receiver = object : BroadcastReceiver(){
  override fun onReceive(context : Context, intent : Intent){
    val action = intent.action
    when(action){
      BluetoothDevice.ACTION_FOUND -> {
        val device : BluetoothDevice = intent.getParcelableExtra(BluetoothDevice.EXTRA_DEVICE)
        devicesArr.add(device)								//recyclerView item추가
      }
    }
  }
}
```





----

참고 사이트

티스토리 : https://doqtqu.tistory.com/166?category=858763

공홈 : https://developer.android.com/guide/topics/connectivity/bluetooth#TheBasics

정보통신기술용어해설(오래된 자료) : http://www.ktword.co.kr/test/view/view.php?no=5376

외국사이트 : https://iotlab.tertiumcloud.com/2020/08/19/classic-bluetooth-vs-bluetooth-low-energy-ble/