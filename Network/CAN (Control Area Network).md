# CAN (Control Area Network)

CAN은 MCU 측면에서 TX와 RX포트를 사용하여 CAN 트랜시버에 CAN High, Can Low를 사용하는 시리얼 기반에 통신이다.

![img](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ae7812b8-ffe5-47fd-93c9-89698f30549d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T122454Z&X-Amz-Expires=86400&X-Amz-Signature=339a54f4b340f4a1f53c19bbc82da7398724bf441d17edac077d04f5081bce29&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

자동차의 ABS, 엔진, 트랜션, 기어박스 등의 인베디드 시스템이 같은 라인에 같은 통신 속도로 연결된다.

모든 노드들은 CAN BUS Line을 통해 데이터 전송 또는 수신을 하게 되는 통신 방식이다.



### CAN의 필요성

- 자동차 노드들 → ECU (Electronic Control Unit) 간의 정보 공유
  - 엔진 ECU
  - 미션 ECU
  - ABS
  - Air-Bag
  - ETACS 등
- 늘어나는 차량의 센서의 공용화 필요
- 스타크 등 노이즈가 많은 차량 환경에서 강인한 통신이 필요해서 독립적인 성향의 ECU들 간의 Network가 요구 된다.



## ISO/OSI 7 Layer Network Reference Model

![img](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1e8eb6d4-41b0-47e7-920d-b4629a267847/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T122549Z&X-Amz-Expires=86400&X-Amz-Signature=0da2c2039832630c04a9b9d57c99d8590af7b9c5d7400087aed36fefdbe61f63&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

Can의 네트워크 모델

- 1 - Physical Layer
- 2 - Data Link Layer
- 3 - 6 Layer (한 덩어리)
- 7 - Application Layer

총 4 Layer로 나누어진다고 보면 된다.

CAN Transceiver

MCU에서 CAN TX, RX를 CAN High, Low로 변경해주는 CAN Transceiver는 1 - 2 계층에 중간에 있다.

### CAN Physical Layer

- CAN High Speed
  - ISO 11989-2
  - 1 Mbps 버스 속도 지원
- CAN Low Speed
  - 폴트 토런스 기능 → 통신선의 문제가 발생할 경우 단일 와이어로도 통신 가능함
  - ISO 11898-3
  - 125 kbps 버스 속도 지원