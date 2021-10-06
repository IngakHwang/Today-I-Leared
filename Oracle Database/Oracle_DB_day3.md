# Oracle Database day3

## SQL CRUD 중 R

### SELECT

> TABLE 내 데이터 조회를 위한 명령어

**`SELECT {컬럼} FROM {테이블} WHERE {조건}` =>table 내 데이터 조회**

```sql
--조건 무(생략) -> table 내 데이터 모두 조회
SELECT * FROM userTBL;

--조건 유 -> 조건에 맞는 데이터 조회
SELECT * FROM userTBL WHERE userName = '김경호';
	--: userTBL table 에서 userName이 김경호인 데이터 모두 조회
	
--AND, BETWEEN
SELECT userName, height FROM userTBL WHERE height >= 180 AND height <= 183;
SELECT userName, height FROM userTBL WHERE height BETWEEN 180 AND 183;
	--: userTBL table 에서 height가 180이상 and 183이하 데이터 중 userName,height 조회
	
--OR, IN
SELECT userName, addr FROM userTBL WHERE addr='경남'OR addr='전남'OR addr='경북';
SELECT userName, addr FROM userTBL WHERE addr IN('경남','전남','경북');
	--: userTBL table 에서 addr이 경남or전남or경북 데이터 중 userName, addr 조회
	
--LIKE
SELECT userName, height FROM userTBL WHERE userName LIKE '김%';
	--: userTBL table 에서 userName이 '김'으로 시작하는 데이터 중 userName,height 조회

SELECT userName, height FROM userTBL WHERE userName LIKE '_종신';
	--: userTBL table 에서 userName이 맨 첫글자 제외 '종신'이 들어간 데이터 중 userName,height 조회 
	
	ex) LIKE '_용%' --> 첫글자 아무거나 두번째글자 용 이고 세번째 이후 몇글자 상관없이 모두 조회 
```



#### 서브쿼리

> 쿼리문 안에 또 쿼리문이 들어 있는 것

> ANY : 서브쿼리의 여러 개의 결과 중 한가지만 만족해도 가능 = SOME

> ALL : 서브쿼리의 여러 개의 결과를 모두 만족시켜야 가능



