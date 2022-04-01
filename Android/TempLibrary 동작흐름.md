# TempLibrary 동작흐름



**MainFragment.kt**

​	initComponent() : view 내 클릭이벤트 정의

​		btnObdiiStart 버튼 : ObdController의 obdiiServiceState(Enum - connect, disconnect)가 disconnect이면 connectObd 함수 실행



**ObdController.kt (object)**

​	connectObd(_activity : Activity, _obdiiMacAddress : String, _vehicleModel : CarInfo) -> CarInfo (Enum)

​		: 입력값 _obdiiMacAddress 비어있는지, 맥 주소인지, 입력값 _vehicleModel이 CarInfo.NONE 인지, GPS가 꺼져있는지 조건 확인 후 

​		: 블루투스 연결? , GPS 연결, DistanceHelper 실행, ObdSettingData.updatePrefData(_acitivity)

​			ObdSettingData.updatePrefData(_acitivity) -> obdmacaddress, driverid, carPlate, vehicleModelId 쉐어드에 저장

​		: internalController.(ObdInternalController 객체) 내 init(), setVehicleType(_vehicleModel.id), startDcmAi() 실행



**ObdInternalController.kt**

​	init() : 코루틴스코프 설정?

​	setVehicleType(i : Int) : 입력값 i와 CarInfo 데이터 매칭 (toOdbCarTyoe, softwareTachoMode 설정)

​		부가설명 : 차가 엔진차인지, 전기차인지, 전기차라면 차종 매핑

​	startDcmAi() : performInitObdii() 실행 및 코루틴 실행?

​	performInitObdii() : dcmAi(DcmAI 객체)의 obdiiServiceState 속성이 obdiiServiceState.DISCONNECT 라면 initOBDii() 실행

​		부가설명 : DcmAI - Data Collection Module Application Interface (어플리케이션 사이드 인터페이스)

​		

**DcmAI.kt**

initOBDii() : _soService(JObdSerVi 객체) 생성 및 startModule() 실행, gatherData() 실행

gatherData(period : Long = 500L) : Disposable? 및 Observable 선언? 



*DcmAI 객체 생성하면서 DcmDI 객체 생성*



​	**JObdSerVi 생성인자** (DcmAI.kt 에 정의되어 있음)

​	performCreate: () -> Unit => createOBDii() : obdiiServiceState 속성을 ObdiiServiceState.CONNECT로 변경

​	performDestory: () -> Unit => destroyOBDii() : _soService.stopModule() 실행(쓰레드 종료 등 정지 이벤트) 및 	obdiiServiceState 속성 DISCONNECT로 변경, 데이터 수집, 송수신 이벤트 정지 메소드 실행

​	onSoConnectionEvent: ((Int) -> Unit)? = null => checkBluetooth(state: Int) : state 30미만은 블루투스 연결 시도, 	state 30은 블루투스 연결에 대한 이벤트 작성?



**JObdSerVi.kt**

startModule() : obdTask (ObdTask 객체 - Thread) 실행, startUpdateTimer(20L) 실행

ObdTask inner class (Thread) : 블루투스 검색

startUpdateTimer(delayTime : Long) : JObdServiTickTimer.runTimer(timerTick()) 실행

timerTick() : int btStat 1~30, 40, 100, 101, 102, 112, 999 에 대한 when 정의 (so파일에 정의된 함수 실행)

​	1~29 : 블루투스 연결 관련 메소드

​	30 : 데이터 수집 메소드 (obdDataReceiveFromSo())



**JObdServiTickTimer.kt (object)**

runTimer(scope : CoroutineScope, delayTime: Long, block: () -> Unit) : 코루틴? 실행



---

탐구필요클래스

DistanceHelper.kt		 	: 정확하게 파악 필요

TrackerModule.class		: GPS 관련 클래스

Bluetooth 관련 클래스(BluetoothAdapter.class)





