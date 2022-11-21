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



Specifically, an ECU can prepare information (for example sensor data) and boradcast this via the CAN bus.

![image-20221121214507106](.\assets\image-20221121214507106.png)



The physical communication happens via the CAN bus wiring harness, consisting of two wires, CAN low and CAN high.



The boradcasted data is accepted by all other ECUs on the CAN network - and each ECU can then check the data and decide whether to receive or ignore it.



In more technical terms, the controller area network is described by a data link layer 

and physical layer.



Specifically, ISO 11898-1 describes the data link layer, while ISO 11898-2 describes the physical layer.



For details, see our CAN bus intro article.



## The top 4 benefits of CAN bus

Today, CAN bus is standard in practically all automotives like cars, trucks, buses, tractors, bikes as well as ships, planes, EV batteries, machinery and more.



There are 4 key benefits to CAN bus that help explain the popularity: 



First, CAN bus is simple and low cost.



ECUs communicate via a single CAN system instead of via direct complex analogue signal lines - reducing errors, weight, wiring and costs.



Second, it is fully centralized.



The CAN bus provides 'one point-of-entry' to communicate with all network ECUs- enabling central diagnostics, data logging and configuration.



Third, CAN is extremely robust.



In particular, the system is robust towards electric disturbances and electromagnetic interference - making it ideal for safety critical applications.



Finally, it is efficient.



CAN frames are prioritized by ID so that top priority data gets immediate bus access, without causing interruption of other frames.



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



Communication over the CAN bus is done via CAN frames.



Here is a standard CAN frame with an 11 bit identifier, which is the type used in most cars.



Let's briefly go through each of the 8 message fields:



Number 1 is the Start of Frame, which is a 'dominant 0', informing the other nodes that the CAN node intends to broadcast information.



Number 2 is the 11-bit or 29-bit CAN ID, which serves to identify the frame.



Lower value IDs have higher priority on the bus.



Number 3 is the Remote Transmission Request field, which indicates whether a node sends data or requests dedicated data from another node.



Number 4 is the 6-bit Control, which contains the Identifier Extension Bit which informs whether the CAN ID is 11-bit or 29-bit.



The Control field also contains the 4 bit Data Length Code (DLC), specifying the length of the data bytes to be transmitted, which can be 0 to 8 bytes.



Number 5 is the Data payload, which contains the actual information to be communicated -for example the front right wheel speed.



We will go into more detail on 'CAN signals' shortly.



Number 6 is the Cyclic Redundancy Check, which is used to ensure the data integrity.



Number 7 is the Acknowledgement slot, which indicates if the node has acknowledged and received the data correctly.



And finally, number 8 is the End Of Frame field that marks the end of the CAN frame.



We've highlighted the CAN ID and payload because these are the most relevant fields in regards to CAN bus data logging, which we'll look at next.



## CAN bus data logging use case examples

To truly understand the CAN bus protocol, we find it uesful to explain how to record and decode CAN bus data.



CAN data acquisition is done across many use cases today.



For example, CAN data from cars can be used to reduce fuel costs, improve driving, test prototype parts - and for insurance optimization.



For more details, see also our OBD2 data logging intro.



Similarly, CAN data from trucks, buses, tractors etc.



can be used in fleet management to reduce costs or improve safety.



For details on this, see our J1939 intro and J1939 telematics intro.



Another increasingly popular use case for CAN data is predictive maintenance.



Vehicles and machinery can be monitored via IoT CAN loggers that communicate data to remote servers.



This data can in turn be used to predict and avoid breakdowns.



Finally, CAN loggers are increasingly being used as blackboxes in vehicles and machinery, providing data for diagnostics and for example warranty dispute handling.



## How to record CAN bus data in practice

But how do you actually record CAN bus data?



As mentioned, two CAN fields are important for CAN logging: The CAN ID and the Data.



7:40



