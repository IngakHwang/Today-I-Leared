# 타입 변환(promotion, casting)

타입 변환 : 데이터 타입을 다른 데이터 타입으로 변환하는 것



## Promotion (자동 타입 변환)

Promotion : 프로그램 실행 도중에 자동적으로 타입 변환이 일어나는 것

**작은 크기를 가지는 타입이 큰 크기를 가지는 타입에 저장 될 때 발생**



큰 크기 타입와 작은 크기 타입의 구분은 사용하는 메모리 크기

byte(1) < short(2) < int(4) < long(8) < float(4) < double(8)



float은 4byte 이지만 int와 long 보다 더 큰 타입으로 표시

그 이유는 표현 할 수 있는 값의 범위가 float이 더 크기 때문

```java
byte byteValue = 10;
int intValue = byteValue; 		//자동 타입 변환이 일어난다.
```



자동 타입 변환이 발생되면 변환 이전의 값과 변환 이후의 값은 동일하다.

즉, 변환 이전의 값은 변환 이후에도 손실 없이 그대로 보존



## Casting (강제 타입 변환)

Casting : 강제적으로 큰 테이터 타입을 작은 데이터 타입으로 쪼개어서 저장하는 것을 Casting이라고 한다.

how? 캐스팅 연산자 () 를 사용, 괄호 안에 들어가는 타입은 쪼개는 단위

```java
int intValue = 103029770;
byte byteValue = (byte) intValue;		//Casting (강제 타입 변환)
```

끝 1byte만 byte 타입 변수에 담게 되므로 원래 int 값은 보존 되지 않는다.



**Casting 할 때 주의할 점**

사용자로부터 입력받은 값을 변환할 때 값의 손실이 발생하면 안된다

정수 타입을 실수 타입으로 변환 할 때 정밀도 손실을 피해야 한다.