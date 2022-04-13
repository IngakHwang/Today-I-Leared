# Android Bluetooth

> 스마트폰, 노트북, 이어폰, 헤드폰 등의 전자기기를 서로 연결하여 종보를 교환하는 근거리 무선 기술 (10M 이내)의 표준



## 원리

블루투스의 원리 = 무선전파의 원리

안테나를 통해 보내고자 하는 데이터를 전자기파로 변조하여 보내고

안테나가 수신하여 전자기파를 데이터로 복조 시키는 것

(복조 : 데이터 통신에서 수신된 신호를 원래의 신호로 복구하는 조작)



## Bluetooth와 BLE 차이

BLE : Bluetooth Low Energy



Bluetooth 4.0 이전에는 Master,Slave 관계를 형성해 통신하는 Bluetooth Classic 방식을 사용했다.

하지만 이 방식은 배터리 소모량이 많아 불편함과 제약이 많았다.

Class 방식보다 훨신 적은 전력으로 비슷한 수준의 통신을 할 수 있게 되었다. 이를 BLE(Bluetooth Low Energy) 라고 부른다.



BLE는 개인용 무선 네트워크 (Wireless Personal Area Network, WPAN) 기술의 일종

기존 블루투스 (Bluetooth Classic, BR/EDR)과는 사실상 별개의 기술



차이점은 블루투스(BR/EDR + HS) 기술이 데이터 전송 속도 (data rate)에 초점을 맞추고 있는 반면,

BLE은 전력 소모를 줄이는 것에 초점을 맞추며 헬스케어, 피트니스, 보안 시스템 등의 특정 응용분야에 적합한 기술로 대두

​	저전력, 저비용, 단순성, 저속도, 소형 지향

​	BLE 로만 동작하는 싱글 모드 (Bluetooth SMART)

​	BLE + 블루투스 모두 지원하는 듀얼 모드 (Bluetooth SMART Ready)

----

Bluetooth Classic은 음악 재생과 같은 연속적인 데이터 스트림을 전송하도록 설계

BLE는 전력 효율성에 최적화



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

  

### Bluetooth 장치 구분

> 블루투스 통신을 사용하는 장치들은 3가지로 구분되지만, 개념적인 구분일 뿐이니 참고만 할 것

- Blueooth Classic : BLE 이전의 스펙을 사용하는 기기를 지칭, 멀티미디어, 대용량 텍스트 등을 다루는 장치들
- Bluetooth Smart Ready : Bluetooth Classic 과 BLE를 모두 지원하는 장치, 핸드폰이나 PC, 태블릿이 해당, 보통 Bluetooth Smart Ready 마크를 붙여 해당 기능을 가지고 있음을 표시
- Bluetooth Smart : BLE 연결만 지원하는 장치, 저전력으로 구동되며 소량의 데이터를 생산하는 센서 장치가 여기에 해당



### BLE 연결 방식

> 크게 2가지

#### Advertise Mode

> 자신의 존재를 알리거나, 적은 양의 Data 보낼 때 사용

![image-20220411174307371](https://tva1.sinaimg.cn/large/e6c9d24egy1h15vik42umj218y0u0mze.jpg)

- Advertiser(Peripheral) : 일정한 주기로 신호를 방송하듯 주변에 뿌리는 디바이스
- Observer(Central) : Advertiser 에게 신호를 받기 위해 주기적으로 Scanning 을 하는 디바이스



기존 Classic 방식은 Master가 주변 Slave 에게 요청을 날리고, Slave가 그에 응답한 뒤 페어링을 시도하는 방식



반면, BLE 는 Peripheral(Slave)가 주변에 일정 간격으로 Advertise를 하고, 

Central(Master) 이 스캔에 성공함으로서 페어링을 하게 된다.

두 디바이스가 연결되면 Advertise Mode는 종료되고, Connection Mode로 1:1 통신을 하게 된다.

Advertise 방식은 말 그래도 signal 을 일방적으로 뿌리는 것이기 떄문에 보안에 취약하다.



#### Connection Mode

> 양 방향 통신을 하거나, Advertise 만으로 많은 양의 데이터를 주고 받을 수 없을 때 사용
>
> 1:1 통신

- Peripheral(Slave) : 연결하기 위한 Advertise 신호를 주기적으로 보내다가, Central 디바이스가 연결 요청을 보내면, 이를 수락해 연결
- Central(Master) : 다른 디바이스의 Advertise 신호를 주기적으로 스캔하다가, 연결을 요청



### Bluetooth Protocol Stack



### ATT/GATT

BLE 프로토콜 스택에서 Attribute protocol (ATT) 은 서버와 클라이언트 사이에 데이터 교환에 대한 규칙을 정의한다.

아래 그림과 같이, 앱 단에서의 데이터 교환은 ATT를 기반으로 이뤄지며 각각의 데이터 구조는 Generic Attribute Profile (GATT)에 의해 정의하는 데이터 구조를 따른다.

> *대두분의 BLE 칩 제작사 는 BLE 프로토콜 스택 소스를 제공하기 때문에, 펌웨어 개발자가 직접 BLE 스택을 프로그래밍 하거나 수정하는 경우는 드물다.*
>
> *다만, BLE 디바이스의 데이터가 어떤 식으로 생성되고, 어떤 정보를 기반으로 데이터 교환하는지 알아두는 것은 전반적으로 BLE 프로토콜을 이해하는 데 큰 도움이 된다.*

![img](https://enidanny.github.io//assets/images/ble-att-gatt.png)



#### GATT

> Generic Attribute Profile
>
> 두 BLE 장치 간에 Service, Characteristic 을 이용해서 데이터를 주고 받는 방법을 정의한 것

두 BLE 장치 간에 Service, Characteristic 을 이용해서 데이터를 주고 받는 방법을 정의한 것

GATT에 의해 정의되는 BLE 시스템의 데이터 구조는 아래 그림과 같이 service와 characteristic으로 표현된다.

> *Service is a collection of information, and Characteristic is a set of actual values and information*
>
> *서비스는 정보의 집합이고 특성은 실제 값과 정보의 집합*

![img](https://enidanny.github.io//assets/images/ble-gatt-structure.png)

**Service**

> A collection of characteristics (data fields) that describes a feature of a device
>
> 장치의 기능을 설명하는 특성(데이터 필드) 모음
>
> 하나의 서비스는 특성들의 집합
>
> 특정 앱을 구현하기 위해 필요한 데이터의 집합

특정 어플리케이션을 구현하기 위해 필요한 데이터의 집합이라고 할 수 있으며, 각각의 데이터는 characteristic에 의해 정의

예를 들어, 심박수 측정 데이터를 관리하기 위한 Service를 정의할 경우,

해당 Service는 실제 심박 수치를 저장하는 characteristic 를 비롯해, 

센서의 단위나 모델 번호, 또는 보안 관련 파라미터 등의 정보를 관리하는 다수의 characteristic 를 포함할 수 있다.



이러한 service와 characteristic 에 대한 정보는 attribute 라는 최소 데이터 유닛에 의해 정의되고, BLE 디바이스 내의 attribute에 대한 정보는 최종적으로 attribute table 내에 저장된다.



각각의 attribute는 다음 3가지 정보를 포함하고 있다. 

- type (UUID)
- handle
- permission



**Attribute Table 예시**

![img](https://enidanny.github.io//assets/images/ble-attribute-table.png)

`handle` 은 attribute의 주소 값에 해당하고,

 `permission` 에는 단어 뜻 그대로 해당 service 또는 characteristic 의 접근 권한에 대한 정보 등이 저장된다.

`UUID` 는 **Universally Unique ID** 의 약자로, GATT 데이터 성분을 구분하기 위한 고유 식별자로 사용된다.



**Service 정의 순서**

`0x2800` 은 Service 선언에 대한 attribute `UUID` 값이다.

위의 Attribute Table 그림에서 맨위 행을 보면, UUID = 0x2800 값을 갖는 attribute 를 이용해 service를 선언하고 있다.

본 예시에서 해당 attribute의 `value` 에는 심박(Heart Rate) 측정 service에 대한 `UUID` 값 `0x180D` 이 저장되어 있다.

---

사전에 정의된 GATT service에 대한 `UUID` 값 https://www.bluetooth.com/specifications/assigned-numbers/

---

다음으로, `0x2803` 은 characteristic 선언에 대한 attribute `UUID` 값이고, 해당 attribute의 `value` 에는 선언하고자 하는 characteristic의 `UUID = 0x2A37` 과 `handle` 값이 저장되어 있다.



`0x2A37` 은 HRS 에서 심박 수 센서 값을 저장하는 characteristic 의 `UUID` 이고, 위의 예시에서 볼 수 있듯 `UUID = 0x2A37`  값을 가지는 attribute의 `value` 에는 `167` 이란 정보가 저장되어 있다.



BLE 디바이스는 하나 이상의 service를 포함할 수 있으며, 각각의 service 또한 하나 이상의 characteristic을 포함한다.

![img](https://enidanny.github.io//assets/images/ble-data-exchange.png)



**GATT Server/Client**

무선 연결된 BLE 디바이스 중 데이터를 가지고 있는 디바이스가 GATT Server

데이터를 요청하는 디바이스가 GATT Client



ex)

스마트폰 앱을 이용해서 BLE 센서 노드의 데이터를 수신하고자 할 때, 

BLE 통신을 시작하는 디바이스는 스마트폰이기에 스마트폰이 GAP central

센서노드는 GAP peripheral

실질적인 데이터를 가지고 있는 BLE 센서 노드는 GATT Server

데이터를 요청하는 스마트폰은 GATT Client



**ATT**

> Attribute Protocol

GATT는 ATT의 최상위 구현체이며 GATT/ATT 참조되기도 한다.

각각의 속성 (Attribute)은 UUID를 가지며 128 비트로 구성

ATT에 의해 부여된 속성은 특성 (characteristic)과 서비스(Service)를 결정



**Characteristic**

> An entity containing meaningful data that can typically be read from or written to
>
> 일반적으로 읽거나 쓸 수 있는 의미 있는 데이터를 포함하는 엔티티

하나의 특성(characteristic)은 하나의 값과 n개의 Descriptor를 포함



**Descriptor**

> A[ defined attribute](https://www.bluetooth.com/specifications/gatt/descriptors/) that describes the characteristic that it’s attached to
>
> 연결된 특성을 설명하는 정의된 속성

특성의 값을 기술



**Notifications**

>  A means for a BLE peripheral to notify the central when a characteristic’s value changes.
>
> 특성 값이 변경되면 BLE 주변 장치가 중앙에서 이를 알리는 수단입니다.



**Indictations**

> Same as an indication, except each data packet is acknowledged by the central. This guarantees their delivery at the cost of throughput.



**UUID**

> Universally unique identifier, 128-bit number used to identify services, characteristics and descriptors.
>
> Service, Characteristics, Descriptors 를 식별하는 데 사용되는 128 Bit 번호



## Bluetooth Class

- **BluetoothAdapter**

  블루투스 송수신 장치를 나타낸다. 블루투스 상호작용에 대한 진입점이다.

  이를 사용해서 다른 블루투스 기기를 검색하고 연결된(페어링된) 기기 목록을 쿼리하고 알려진 MAC 주소로 `BluetoothDevice` 를 인스턴스화하고, 다른 기기로 부터 통신을 수신 대기하는 `BluetoothServerSocket` 을 만들 수 있다.

- **BluetoothDevice**

  원격 블루투스 기기를 나타낸다.

  이를 사용해서 `BluetoothSocket` 을 통해 원격 기기와의 연결을 요청하거나 이름, 주속, 클래스 및 연결 상태와 같은 기기 정보를 쿼리한다.

- **BluetoothSocket**

  블루투스 소켓에 대한 인터페이스

  `InputStream`, `OutputStream` 을 통해 앱이 다른 블루투스 기기와 데이터를 교환할 수 있게 허용하는 연결 지점

- **BluetoothServerSocket**

  들어오는 요청을 수신 대기하는 OpenServerSocket

  두 대의 Android 기기를 연결 시에 한 기기가 이 클래스를 사용하여 서버 소켓을 열어야 한다.

  원격 블루투스 기기가 이 기기로 연결 요청을 보내면 해당 기기가 연결을 수락한 다음, 연결된 `BluetoothSocket` 을 반환

- **BluetoothClass**

  블루투스 기기의 일반적인 특징 및 기능에 대해 설명

  기기 클래스와 서비스를 정의하는 읽기 전용 속성 집합이다.

  이 정보는 기기의 유형에 관한 유용한 힌트를 제공한다.

  다만 이 클래스의 특성만으로 기기가 지원하는 모든 블루투스 프로필과 서비스가 설명되는 것은 아님

- **BluetoothProfile**

  Bluetooth 프로필을 나타내는 인터페이스

  Bluetooth 프로필은 기기 간 블루투스 기반 통신에 대한 무선 인터페이스 사양이다.

  - BluetoothProfile.ServiceListener

    특정 프로필을 실행하는 내부 서비스와 연결하거나 연결을 끊을 때 `BluetoothProfile` IPC 클라이언트에 알리는 인터페이스

- **BluetoothGatt**

  Bluetooth Smart 또는 Smart Ready 장치와 통신할 수 있도록 Bluetooth GATT 기능을 제공

  주변 장치에 연결하려면 이 클래스의 인스턴스를 가져오기 위해 `BluetoothGattCallback` 호출하고 생성

- **BluetoothGattServer**

  Bluetooth GATT Server 역할 기능을 제공하여 응용 프로그램이 Bluetooth Smart 서비스 및 특성을 생성

  BluetoothGattServer는 IPC를 통해 Bluetooth 서비스를 제어하기 위한 프록시 개체

- **BluetoothGattCallback**

  추상 클래스로 BluetoothGattCallback 구현

  - onCharacteristicChanged : 원격 characteristic 알림 결과 트리거된 콜백
  - onCharacteristicRead : characteristic 읽기 작업의 결과를 보고하는 콜백
  - onCharacteristicWrite : characteristic 쓰기 작업의 결과를 나타내는 콜백
  - onConnectionStateChange : GATT 클라이언트가 원격 GATT 서버에 연결/해제 되었을 때를 나타내는 콜백
  - onServiceChanged : Service 변경 이벤트가 수신되었음을 나타내는 콜백, 이 이벤트를 수신한다는 것은 GATT 데이터베이스가 원격 장치와 동기화되지 않았음을 의미
  - onServicesDiscovered : 원격장치에 대한 원격 service, characteristic 및 descriptor의 목록이 업데이트 되었을 때, 즉 새로운 서비스가 발견되었을 때 호출되는 콜백



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

임베디드 개발장이 : https://enidanny.github.io/ble/ble-att-gatt/