## 2021. 04. 15.



# 6. PL/SQL 기본

- 데이터베이스에서 사용되는 일종의 공통 언어



### 6.1.3 GROUP BY 및 HAVING 그리고 집계 함수

그룹으로 묶어주는 역할.

`GROUP BY` 없이는 별도의 이름 열을 집계 함수와 같이 사용할 수 없음.

```
SELECT userName, MAX(height),MIN(height) FROM userTBL;
```

​	-> 오류가 나옴.



```
SELECT userID, amount FROM buyTBL ORDER BY userID;
```

- 위 처럼 하면, userID와 amount에 따라 정렬할 뿐.
- 한 명의 user가 총 몇 개의 amount값을 갖는 지를 계산하기 위해 `GROUP BY` 사용.

```
SELECT userID, SUM(amount) FROM buyTBL GROUP BY userID;
```

- `AS` 를 사용해서 결과를 보기 편하게 만들 수 있음. 

```
SELECT userID AS "사용자 아이디", SUM(amount) AS "총 구매 개수" FROM buyTBL GROUP BY userID;
```



#### 집계함수

`GROUP BY` 없이 사용 불가.

- AVG() : 평균을 구한다.
- MIN() : 최솟값을 구한다.
- MAX() : 최대값을 구한다.
- COUNT() : 행의 개수를 센다.
- COUNT(DISTINCT) : 행의 개수를 센다(중복은 1개만 인정).
- STDEV() : 표준편차를 구한다.
- VARIENCE() : 분산을 구한다.



- CAST() : 소수점 조절

  ```
  CAST(AVG(amount) AS NUMBER(5,3))
  ```



#### 가장 키가 큰 회원과 작은 회원 출력 쿼리

`GROUP BY`  말고 서브쿼리로 조합하는 것이 수월.

```
SELECT userName, height
	FROM userTBL
	WHERE height = (SELECT MAX(height)FROM userTBL)
		OR height = (SELECT MIN(height)FROM userTBL);
```

근데 왜 여기선 `GROUP BY` 를 안 해도 오류가 안 날까?

-> userName에 MAX를 물어본 것 자체가 문제였었음. 작동 방식이 다르니까. 근데 height은 데이터형이 숫자이기 때문에 MAX, MIN 값을 끄집어 낼 수 있음.



#### Having절

사용자 별 총 구매액

```
SELECT userID AS "사용자", SUM(price*amount) AS "총 구매액" 
	FROM buyTBL 
	GROUP BY userID;
```

이 중에서 총 구매액이 1,000 이상인 사용자에게 사은품 증정하고 싶을 때, `WHERE` 구문을 떠올려 사용한다면,

```
SELECT userID AS "사용자", SUM(price*amount) AS "총 구매액" 
	FROM buyTBL
	WHERE SUM(price*amount) > 1000
	GROUP BY userID;
```

오류가 발생한다.

이때 사용하는 것이 `HAVING` 절.

`WHERE` 과 비슷한 개념으로 조건을 제한하는 것이지만, 집계 함수에 대해서 조건을 제한하는 것이라 생각하면 됨.

```
SELECT userID AS "사용자", SUM(price*amount) AS "총 구매액" 
	FROM buyTBL
	GROUP BY userID
	HAVING SUM(price*amount) > 1000;
```

여기에 추가로 총 구매액이 적은 사용자부터 나타내려면 `ORDER BY` 사용.

```
SELECT userID AS "사용자", SUM(price*amount) AS "총 구매액" 
	FROM buyTBL
	GROUP BY userID
	HAVING SUM(price*amount) > 1000;
	OREDER BY SUM(price*amount);
```



### 6.1.5 SQL의 분류

크게 DML, DDL, DCL로 분류함.



#### DML(Data Manipulation Language) : 데이터 조작 언어

- 데이터를 조작(선택, 삽입, 수정, 삭제)하는 데 사용되는 언어.
- DML 구문이 사용되는 대상은 테이블의 행이므로 DML 사용 위해선 그 이전에 테이블이 정의되어 있어야 함.
- SELECT, INSERT, UPDATE, DELETE
- 트랜잭션 발생 : 테이블의 데이터 변경(삽입, 수정, 삭제)할 때 임시로 적용시키는 것.
- COMMIT, ROLLBACK



#### DDL(Data Definition Language) : 데이터 정의 언어

- 데이터베이스, 테이블, 뷰, 인덱스 등의 데이터베이스 개체를 생성/삭제/변경하는 역할.
- CREATE, DROP, ALTER 등
- 트랜잭션 발생시키지 않음. -> 실행 즉시 Oracle에 적용.
- DBA의 역할.



#### DCL(Data Control Language) : 데이터 제어 언어

- 사용자에게 어떤 권한을 부여하거나 빼앗을 때 주로 사용하는 구문
- GRANT/REMOVE/KEY 등이 해당.

- DBA의 역할.





## 6.2 데이터의 변경을 위한 SQL문



### 6.2.1 데이터의 삽입 : INSERT



#### INSERT문 기본

테이블에 데이터를 삽입하는 명령어.

```
INSERT INTO 테이블[(열1, 열2, ...)] VALUES (값1, 값2, ...);
```

주의사항

- 테이블 이름 다음에 나오는 열은 생략이 가능. 단, 그러면 VALUE 다음에 나오는 값들의 순서 및 개수가 테이블에 정의된 열 순서 및 개수와 동일해야 함.



#### DEFAULT

만약 내가 데이터를 집어넣는 것을 잊어버리면, 이 값을 넣어라 하고 `CREATE` 에서 `DEFAULT` 를 지정할 수 있음.

NULL 과의 차이 : `NULL`은  `INSERT` 구문을 써야하고, `DEFAULT`는 `UPDATE` 구문 사용해야 함.



#### 자동으로 증가하는 시퀀스

값을 자동으로 증가시켜주는 시퀀스 -> 유일 키로 활용 가능

```
CREATE SEQUENCE idSEQ
	START WITH 1
	INCREMENT BT 1;
```

- 밑에 두 줄 생략 가능

- 시퀀스를 입력하려면 `"시퀀스이름.NEXTVAL"` 을 사용하면 됨.

- 시퀀스 현재 값을 확인하려면 다음과 같이 사용

  ```
  SELECT idSEQ.CURRVAL FROM DUAL;
  ```

  - DUAL 테이블은 Oracle에 내장된 가상의 테이블. 구문의 형식상 FROM이 반드시 있어야 하므로 그냥 뒤에 써주는 것.
  - (100 X 100)을 하고 싶을 때 `SELECT 100*100 FROM DUAL` 문으로 사용하면 됨.

- `CYCLE` : 반복 설정 (KEY 사용 시엔 불필요)
- `NOCACHE` : 캐시 사용 안 함.



#### 대량의 샘플 데이터 생성

`INSERT INTO ... SELECT` 구문





### 6.2.2 데이터의 수정 : UPDATE

기존 입력되어 있는 값을 변형하기 위해. 

실수했을 때 `ROLLBACK;` 명령으로 되돌리자.

```
UPDATE 테이블이름
	SET 열1=값1, 열2=값2 ...
	WHERE 조건;
```

- 여기서 SET은 Java에서 LIST 수정과 유사.
- WHERE 절 정말 중요. 이게 없으면 테이블 내 모든 내용이 수정돼버림.

- 근데 만약 전체 테이블의 내용을 변경하고 싶으면 WHERE 없이 사용하면 간단.



### 6.2.3 데이터의 삭제 : DELETE FROM

UPDATE와 거의 비슷한 개념. 행 단위로 삭제. 

```
DELETE FROM 테이블이름 WHERE 조건 ;
```

- 조건에 해당하는 내용이 포함된 행 전부 삭제

- `DROP` : 테이블 전체 삭제

  ```
  DROP TABLE bigTbl2;
  ```

- `TRUNCATE` : 테이블 삭제. DELETE와 효과 동일. 롤백 불가,

  ```
  TRUNCATE TABLE bigTbl3
  ```

   

### 6.2.4 조건부 데이터 변경 : MERGE

하나의 문장에서 경우에 따라서 INSERT, UPDATE, delete를 수행할 수 있는 구문.

ex)

 멤버 테이블에는 기존 회원들이 존재하는데, 이 멤버 테이블은 중요한 테이블이라 실수를 하면 안되므로 직접 INSERT, DELETE, UPDATE를 사용하면 안 됨. 그래서 회원의 가입, 변경, 탈퇴가 생기면 변경 테이블에 INSERT문으로 회원의 변경사항을 입력함. 변경 테이블의 변경사항은 신규가입/주소변경/회원 탈퇴 3가지의 경우가 있음.

 변경 테이블에 쌓인 내용은 1주일마다, MERGE 구문을 사용해서 멤버 테이블에 적용.

