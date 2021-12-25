# Oracle Datebase day1

## 데이터베이스(Database, DB)

> **데이터의 집합**



### 특징

데이터의 무결성 => 데이터에 오류가 있으면 안된다.

데이터의 독립성 => 타의에 의해 영향을 받으면 안된다.

보안 => 데이터에 아무나 접근하면 안된다.

데이터 중복의 최소화 => 중복저장방지

응용프로그램 제작 및 수정이 쉬워짐

데이터의 안전성 향상 => 백업 가능

## DBMS(DateBase Management System)

> **DB를 잘 관리하고 운영하기 위한 소프트웨어**



DBMS 데이터 구축, 관리, 활용하는 언어 => SQL

### 왜? DBMS를 사용하는가?

컴퓨터가 없을 떄 => 오프라인(문서)로 관리

컴퓨터 사용 => 파일시스템(엑셀,txt 등)으로 관리 => 파일중복 시 수정이 까다로울 수 있음

데이터베이스 관리시스템(RDBS) => 파일시스템의 단점(파일중복)을 보완하고 대량의 데이터를 효율적으로 관리하고 운영하기 위해 사용

### DBMS의 분류

- 계층형(Hierarchical)
- 망형(Network)
- 관계형(Relational) =>DBMS 중 가장 많은 부분 차지
  - 테이블로 이루어져 있으며, 이 테이블은 키와 값의 관계를 나타냄
  - 데이터의 종속성을 관계로 표현하는 것
  - 이렇게 나뉜 테이블의 관계를 기본 키(Primary Key)와 외래 키(Foreign Key)를 사용해서 맺어 줌으로써, 두 테이블을 부모와 자식 관계로 묶을 수 있음
- 객체지향형(Object-Oriented)
- 객체관계형(Object-Relational)

## SQL(Structured Query Language)

> **DB에서 데이터를 정의, 조작, 제어하기 위해 사용하는 언어**



### 명령어

**`CREATE {DATEBASE or TABLE} {이름};` => DB or table 생성**

```sql
CREATE DATABASE exDB;


--table은 필드명, 데이터 유형기재해서 선언해야 함
CREATE TABLE exTable(
	id NUMBER(4),
	userName NCHAR(3),
	age NUMBER(2)
);

```

**`INSERT INTO TABLE[(열1,열,2...)]VALUES(값1,값2...)` =>table 내 데이터 입력**

`INSERT INTO 컬럼명 생략`

```sql
--table(열) 생략 가능
--생략 시 , VALUES 값 순서 및 개수를 TABLE에 정의된 순서와 개수 동일하게 입력

INSERT INTO testTBL1 VALUES(1,'홍길동',25);

INSERT INTO testTBL1(id, userName) VALUES(2,'설현');

--열의 순서를 바꿔서 입력가능
INSERT INTO testTBL1(userName, age, id) VALUES('지민',26,3);

```

### Date Type

#### 문자데이터 타입

| 데이터 타입 | 설명                                          |
| ----------- | --------------------------------------------- |
| CHAR(n)     | 고정길이 - 문자                               |
| VARCHAR2(n) | 가변길이 - 문자                               |
| NCHAR(n)    | 고정길이 - 유니코드 문자(다국어 입력가능)     |
| NVARCHAR(n) | 가변길이 - 유니코드 문자(다국어 입력가능)     |
| LONG        | 가변길이 - 문자 - 최대 2GB크기                |
| CLOB        | 대용량 텍스트 데이터 타입 - 최대 4GB          |
| NCLOB       | 대용량 텍스트 유니코드 데이터 타입 - 최대 4GB |

#### 숫자데이터 타입

| 데이터 타입   | 설명                                                     |
| ------------- | -------------------------------------------------------- |
| NUMBER(P,S)   | 가변숫자 / P(1 ~ 38, 디폴트 : 38) / S(-84~127, 디폴트:0) |
| FLOAT(P)      | NUMBER의 하위타입 / P(1~128, 디폴트:128)                 |
| BINARY_FLOAT  | 32비트 부동소수점                                        |
| BINARY_DOUBLE | 64비트 부동소수점                                        |

#### 날짜데이터 타입

| 데이터 타입 | 설명                                                       |
| ----------- | ---------------------------------------------------------- |
| DATE        | BC 4712년 1월 1일 ~ 9999년 12월 31일, 연월일시분초입력가능 |
| TIMESTAMP   | 연,월,일,시,분,초+밀리초까지 입력가능                      |

