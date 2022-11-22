# CSS CAN Bus Explained

Link : https://www.csselectronics.com/pages/can-bus-simple-intro-tutorial



## What is CAN bus

CAN bus is the nervous system, enabling communication between different parts of the body.

CAN 버스는 차량의 부품간의 통신을 가능하게 하는 신경계이다.



Communication happens between different CAN bus ‘nodes’, which are also called ‘electronic control units’ (or ECUs).

서로 다른 CAN 버스 , ECU라고 불리는 '노드' 간에 통신이 발생한다.



These CAN nodes are like parts of the body, interconnected via the CAN bus.

이 CAN 노드들은 CAN 버스를 통해 상호 연결된 신체의 일부와 같다.



Information from one node can be shared with other nodes.

한 노드의 정보는 다른 노드들과 공유 할 수 있다.



In an automoitve CAN bus system, ECUs can be things like the engine control unit, airbags, audio system etc.

자동차 CAN 버스 시스템에서 ECU는 엔진 컨트롤 유닛, 에어백, 오디오 시스템 등이 될 수 있다.



A modren car may have up to 70 ECUs - and each of them may have information that needs to be shared with other parts of the network.

최근 차량들은 70개 정도 ECU들을 가지고 있을 수 있으며, 각 ECU에는 네트워크의 다른 부분과 공유해야 하는 정보가 있을 수 있다.



This is where the CAN standard comes in handy.

이는 CAN 표준이 유용한 부분이다.



The CAN bus system enables each ECU to communicate with all other ECUs - without complex dedicated wiring.

CAN 버스 시스템은 각각 ECU가 복잡한 전용선 없이 다른 모든 ECU들과 통신할 수 있다.



Specifically, an ECU can prepare information (for example sensor data) and broadcast this via the CAN bus.

정확하게, ECU는 정보(예: 센서 데이터)를 준비하고 CAN 버스를 통해 이를 브로드캐스트 할 수 있다.

![image-20221121214507106](.\assets\image-20221121214507106.png)



The physical communication happens via the CAN bus wiring harness, consisting of two wires, CAN low and CAN high.

CAN 버스 와이어링 하네스를 통해 CAN Low, CAN High의 두 와이어로 구성된 물리적 통신이 이루어 진다.

---

Wiring harness - 와이어링 하네스

자동차 및 전자제품의 각 부위에서 발생되는 전기적 신호 및 전류를 부품 상호간에 전달하여 각 시스템이 제 역할을 수행할 수 있도록 하는 배선의 총 집합체

----



The boradcasted data is accepted by all other ECUs on the CAN network - and each ECU can then check the data and decide whether to receive or ignore it.

브로드캐스트된 데이터는 CAN 네트워크의 다른 모든 ECU에서 승인되며, 각 ECU는 데이터를 확인하고 수신 또는 무시 할지 여부를 결정 할 수 있다.



In more technical terms, the controller area network is described by a data link layer and physical layer.

더 기술적인 용어로, 제어기 영역 네트워크는 데이터 링크 계층 및 물리 계층에 의해 설명된다.



Specifically, ISO 11898-1 describes the data link layer, while ISO 11898-2 describes the physical layer.

구체적으로, ISO 11898-1은 데이터 링크 계층을 설명하고, ISO 11898-2는 물리 계층을 설명한다. 



## The top 4 benefits of CAN bus

Today, CAN bus is standard in practically all automotives like cars, trucks, buses, tractors, bikes as well as ships, planes, EV batteries, machinery and more.

오늘날 CAN 버스는 선박, 비행기, EV 배터리, 기계 등과 같은 자동차, 트럭, 버스, 트랙터, 자전거와 같은 실질적으로 모든 자동차에서 표준이다.



There are 4 key benefits to CAN bus that help explain the popularity: 

CAN 버스의 인기를 설명하는 데 도움이 되는 4가지 Key들



First, CAN bus is simple and low cost.

첫째, CAN 버스는 단순하고 저렴하다.



ECUs communicate via a single CAN system instead of via direct complex analogue signal lines - reducing errors, weight, wiring and costs.

ECU는 직접적인 복잡한 아날로그 신호 라인 대신 단일 CAN 시스템을 통해 통신하므로, 오류, 중량, 배선 및 비용이 절감된다.



Second, it is fully centralized.

둘째, 중앙 집중적이다.



The CAN bus provides 'one point-of-entry' to communicate with all network ECUs- enabling central diagnostics, data logging and configuration.

CAN 버스는 모든 네트워크 ECU와 통신할 수 있는 '원 엔트리 포인트'를 제공하여 중앙 진단, 데이터 로깅 및 구성을 활성화 한다.



Third, CAN is extremely robust.

셋째, CAN은 극도로 탄탄하다.



In particular, the system is robust towards electric disturbances and electromagnetic interference - making it ideal for safety critical applications.

특히, 이 시스템은 전기 장애 및 전자기 간섭에 강하므로 안전에 중요한 애플리케이션에 이상적이다.



Finally, it is efficient.

마지막으로, CAN은 효율적이다.



CAN frames are prioritized by ID so that top priority data gets immediate bus access, without causing interruption of other frames.

CAN 프레임은 ID별로 우선순위가 지정되므로 다른 프레임의 중단 없이 최상위 우선순위 데이터가 즉시 버스에 액세스 할 수 있다.



## The history of CAN bus

In terms of the history of CAN bus, it was initially developed by Bosch in 1986 as a solution to the prior challenge of complex point-to-point wiring in cars.



In 1991, Bosch published CAN 2.0, with 2.0A referring to 11-bit CAN IDs and 2.0B referring to 29-bit CAN IDs.



In 1993, CAN was adopted as an international standard, ISO 11898.



In 2003, the standard was separated for the data link and physical layers.



In 2012, Bosch released the next generation of CAN called CAN FD or CAN with flexible data-rate.



CAN FD was standardized in 2015 and is gradually being rolled out in vehicles and machinery today.



More recently, work has also begun on the next generation CAN XL protocol.



## The CAN frame incl. the 8 message fields

To further understand how CAN bus works, let's look at the CAN frame.

CAN 버스의 작동 방식을 자세히 이해하기 위해 CAN 프레임을 살펴보자.



Communication over the CAN bus is done via CAN frames.

CAN 버스를 통한 통신은 CAN 프레임을 통해 이루어진다.



Here is a standard CAN frame with an 11 bit identifier, which is the type used in most cars.

여기 대부분의 자동차에 사용되는 11비트 식별자가 있는 표준 CAN 프레임이 있다.

![image-20221122213529762](.\assets\image-20221122213529762.png)



Let's briefly go through each of the 8 message fields:

각 8개 메시지 필드에 대해 간단히 살펴보자.



Number 1 is the Start of Frame, which is a 'dominant 0', informing the other nodes that the CAN node intends to broadcast information.

1번은 CAN 노드가 정보를 브로드캐스트하려는 의도를 다른 노드에 알리는 '도미넌트 0' 인 `SOF` Start Of Frame 이다.



Number 2 is the 11-bit or 29-bit CAN ID, which serves to identify the frame.

2번은 프레임을 식별하는 11비트 또는 29비트 CAN ID 이다.



Lower value IDs have higher priority on the bus.

낮은 값의 ID 일수록 버스에서 더 높은 우선 순위를 가진다.



Number 3 is the Remote Transmission Request field, which indicates whether a node sends data or requests dedicated data from another node.

3번은 `RTR` Remote Transmisson Request 필드로, 노드가 데이터를 전송할지 아니면 다른 노드에서 전용 데이터를 요청할지를 나타낸다.



Number 4 is the 6-bit Control, which contains the Identifier Extension Bit which informs whether the CAN ID is 11-bit or 29-bit.

4번은 `6-bit Control`로, CAN ID가 11비트인지  29비트인지 알려주는 식별자 확장 비트를 포함한다.



The Control field also contains the 4 bit Data Length Code (DLC), specifying the length of the data bytes to be transmitted, which can be 0 to 8 bytes.

이 `Control` 필드에는 전송할 데이터 바이트의 길이(0~8 바이트)를 지정하는 4비트 데이터 길이 코드(DLC)도 포함되어 있다.



Number 5 is the Data payload, which contains the actual information to be communicated -for example the front right wheel speed.

5번은 데이터 페이로드로, 전달할 실제 정보(예: 우측 휠 속도)가 포함되어 있다.



We will go into more detail on 'CAN signals' shortly.

이 후에 CAN 신호에 대해 자세히 알아보자.



Number 6 is the Cyclic Redundancy Check, which is used to ensure the data integrity.

6번은 데이터 무결성을 보장하기 위해 사용되는 `CRC` Cyclic Redundancy Check (주기적 중복 검사)이다.



Number 7 is the Acknowledgement slot, which indicates if the node has acknowledged and received the data correctly.

7번은 `ACK` Acknowledgement 슬롯으로, 노드가 데이터를 올바르게 승인하고 수신했는 지 여부를 나타낸다.



And finally, number 8 is the End Of Frame field that marks the end of the CAN frame.

마지막 8번은, CAN 프레임의 끝을 표시하는 `EOF` End Of Frame 이다.



We've highlighted the CAN ID and payload because these are the most relevant fields in regards to CAN bus data logging, which we'll look at next.

CAN ID와 데이터 페이로드는 CAN 버스 데이터 로깅과 관련하여 관련성이 높은 필드이다.



## CAN bus data logging use case examples

To truly understand the CAN bus protocol, we find it uesful to explain how to record and decode CAN bus data.

CAN 버스 프로토콜을 진정으로 이해하려면 CAN 버스 데이터를 기록하고 디코딩하는 방법을 설명하는 것이 유용하다.



CAN data acquisition is done across many use cases today.

CAN 데이터 수집은 오늘날 많은 사용 사례에서 수행된다.



For example, CAN data from cars can be used to reduce fuel costs, improve driving, test prototype parts - and for insurance optimization.

예를 들어, 자동차의 CAN 데이터는 연료 비용 절감, 주행 개선, 시제품 부품 테스트 및 보험 최적화에 사용될 수 있다.



For more details, see also our OBD2 data logging intro.



Similarly, CAN data from trucks, buses, tractors etc.



can be used in fleet management to reduce costs or improve safety.

비용을 절감하거나 안전성을 개선하기 위해 비행대 관리에 사용될 수 있다.



For details on this, see our J1939 intro and J1939 telematics intro.



Another increasingly popular use case for CAN data is predictive maintenance.

CAN 데이터의 또 다른 널리 사용되는 사용 사례는 예측 유지보수이다.



Vehicles and machinery can be monitored via IoT CAN loggers that communicate data to remote servers.

원격 서버에 데이터를 전달하는 IoT CAN 로거를 통해 차량과 기계를 모니터링할 수 있다.



This data can in turn be used to predict and avoid breakdowns.

이 데이터는 고장을 예측하고 방지하는 데 사용될 수 있다.



Finally, CAN loggers are increasingly being used as blackboxes in vehicles and machinery, providing data for diagnostics and for example warranty dispute handling.

마지막으로, CAN 로거는 차량 및 기계에서 블랙박스로 점점 더 많이 사용되고 있으며, 진단 및 보증 분쟁 처리를 위한 데이터를 제공한다.



## How to record CAN bus data in practice

But how do you actually record CAN bus data?



As mentioned, two CAN fields are important for CAN logging: The CAN ID and the Data.



7:40



