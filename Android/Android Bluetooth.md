# Android Bluetooth



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