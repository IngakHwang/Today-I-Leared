# UML (Unified Modeling Language)

> 프로그램 설계를 표현하기 위해 사용하는 표기법



- 프로그램 설계를 표현하기 위해 사용하는 표기법
- 요구분석, 시스템 설계, 시스템 구현 등의 시스템 개발 과정에서 개발자간의 의사소통을 원활하게 이루어지게 하기 위하여 표준화한 모델링 언어
- 소프트웨어 시스템, 업무 모델링, 시스템의 산출물을 규정하고 시각화, 문서화하는 언어
- 프로그램언어가 아닌 기호와 도식을 이용하여 표현하는 방법을 정의한다.
- 모델링 언어 O, 방법론 X , 프로그래밍 언어 X
- 작성 목적 : 객체 지향 시스템을 가시화, 명세화, 문세화



## unified?

UML 등장 이전에 Booch, OMT, OOSE 등 다양한 객체 모델링 방법이 공존 했는데, 서로 다른 방법을 사용한 조직 간의 정보를 공유하려면 다른 표현 방식을 익혀야 했다.



이러 불편한을 해소하기 위해 다음과 같은 목표로 통합하게 되었다.

1. 객체지향 개념을 이용하여 소프트웨어 영역뿐만 아니라 시스템 영역도 모델링 할 수 있게 한다.
2. 실행 가능하거나 개념적인 산출물들을 명확하게 결합할 수 있게 한다.
3. 사람과 기계에 모두 유용한 모델링 언어를 만든다.



**목표**

- 사용자들이 의미있는 모델을 만들고 공유할 수 있도록 사용하기 쉬우면서 표현이 풍부한 시각적 모형화 언어를 제공한다. (즉, 문서의 직관성을 높인다.)
- 핵심 개념을 확장하기 위한 메커니즘을 제공한다.
- 특정 프로그래밍 언어나 개발 공종에 종속되지 않아야 한다. (통합성을 중요시한다.)
- 모델링 언어를 이해하기 위한 공식적 기준을 제공한다.
- 객체지향 도구 시장의 성장을 장려해야 한다.
- 협동, 프레임워크, 패턴, 컴포넌트 등의 고수준의 개발 개념들을 지원한다.
- 산업계의 검증된 최상의 경험들을 통합한다.



## UML 특징



### 가시화 언어

소프트웨어의 개념 모델을 시작적인 그래픽 형태로 작성한다.

표기법에 있어서는 Symbol에 명확한 정의가 존재하므로 개발자들 사이에서 원활한 의사소통이 가능하다.



### 명세화 언어

명세화란 정확하고 명백하여, 완전한 모델을 만드는 것을 의미한다.

UML은 소프트웨어 개발과정인 분석, 설계, 구현 단계의 각 과정에서 필요한 모델을 명세화 할 수 있는 언어이다.

개발 단계 초기부터 어떤 input값이 들어갈 것인지, 어떤 Process를 통해서 ouput이 나올 것인지를 모두 확실하게 모델링하여 개발 단계에 들어갔을 때 이 명세서를 따라 그대로 개발 할 수 있도록 한다.



### 구축 언어

UML로 명세화된 설계모델은 Java, C++ 등 다양한 언어의 소스코드로 변환하여 구축 할 수 있다.

반대로 구축되어 있는 소스코드를 UML로 변환하여 분석하는 Reverse 도 가능하다.

가시화, 명세화 된 모델을 실제로 동작하는 소스코드로 변환하여 구축하는 것이다.



### 문서화 언어

시스템 아키텍처와 이에 대한 모든 상세 내역에 대한 문서화를 다루며, 요구사항을 표현하고 시스템을 테스트하는 언어도 제공한다. 일련의 과정을 문서로 남겨 계속 유지 보수한다.



## UML 요소

> https://onecellboy.tistory.com/334



> - Thing
>   - Structual Thing
>   - Behavioral Thing
>   - Grouping Thing
>   - Annotation Thing
> - Relationships
>   - Dependency Relationship
>   - Generalization Relationship
>   - Association Relationship
>   - Aggregation Relationship
>   - Composition Relationship
>   - Realization Relationship
> - Diagrams
>   - Class Diagram
>   - Object Diagram
>   - Use Case Diagram
>   - Sequence Diagram
>   - Collaboration Diagram
>   - State Diagram
>   - Activity Diagram
>   - Component Diagram
>   - Deployment Diagram





----

참고 사이트 : https://velog.io/@hanblueblue/UML-UML-%EA%B8%B0%EC%B4%88