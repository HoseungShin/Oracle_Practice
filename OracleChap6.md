## 2021. 04. 15.



# 6. PL/SQL 기본

- 데이터베이스에서 사용되는 일종의 공통 언어



## 6.1 SELECT 문 

- SELECT / INSERT / UPDATE / DELETE

  

### 6.1.1 원하는 데이터를 가져와 주는 기본적인 <SELECT ... FROM>

- 데이터베이스 내의 테이블에서 원하는 정보를 추출하는 명령 



#### SELECT의 구문 형식

```
SELECT 열 이름
FROM 테이블 이름
WHERE 조건;
```



#### SELECT 와 FROM

```
SELECT * FROM employees;
```



### 테이블 생성

```
CREATE TABLE (테이블 이름)
( 이름 데이터 타입(크기) 널 유무 (제약사항),
  열 추가
  열 추가,
);
```

- PRIMARY KEY에는 NOT NULL이 포함돼 있어서 NOT NULL 굳이 안 적어도 됨

- NOT NULL : 반드시 데이터가 들어와야 한다.

- 외래 키 설정

  ``` 
  FOREIGN KEY (외래키로 설정할 데이터 이름) REFERENCES 데이터있는테이블(데이터 이름)
  ```

  ex)

  ```
  FOREIGN KEY (userID) REFERENCES userTBL(userID)
  ```



#### 테이블에 데이터 입력

````
INSERT INTO 테이블이름 VALUES(A, B, C, D, E, F, G, H);
````

- 데이터 타입(크기) 에 맞게끔 VALUES 안에 차례로 값 기입

```
CREATE SEQUENCE 시퀀스이름;
```

- 시퀀스 : 순차적으로 값이 증가하는 데이터베이스 개체. 



시퀀스 이용해서 데이터 입력 ex)

```
INSERT INTO buyTBL VALUES(idSEQ1.NEXTVAL, 'KBS', '운동화', NULL, 30, 2);
```

위 내용중 idSEQ1이 차례로 순서를 부여하는 시퀀스의 역할을 함



### 6.1.2 특정한 조건의 데이터만 조회하는 <SELECT ... FROM ... WHERE>

- 조회하는 결과에 특정한 조건을 줘서, 원하는 데이터만 보고 싶을 때 사용.

````
SELECT 필드이름 FROM 테이블이름 WHERE 조건식;
````

ex)

```
SELECT * FROM userTBL WHERE username = '김경호';
```



#### 관계 연산자의 사용

- AND : 이고
- OR : 또는, 이거나
- BETWEEN ... AND : 연속적 값을 가지고 있을 때  `BETWEEN 180 AND 183;`
- IN() : 연속적인 값이 아닌 이산적인 값일 때  `IN ('경남', '전남', '경북');`
- LIKE : 문자열의 내용 검색 위해 사용  `LIKE '김%';`
- %  :  무엇이든 허용. 즉, '김%' 의 경우엔 '김'이 제일 앞 글자인 모든 것을 추출.
- _ : 한 글자와 매치해 허용. 즉, '_종신' 의 경우엔 맨 앞 글자가 한 글자이고 그 다음이 '종신'인 사람 조회.



#### ANY/ALL/SOME 그리고 서브쿼리(SubQuery, 하위쿼리)

쿼리문 안에 또 쿼리문이 들어 있는 것.

```
SELECT userName, height FROM userTBL WHERE height > 177;
```

에서 177이라는 키를 직접 써 주는 것이 아니라, 이것도 쿼리를 통해서 사용하려는 것.

```
SELECT userName, height FROM userTBL 
	WHERE height > (SELECT height FROM userTBL WHERE userName = '김경호');
```

서브쿼리가 둘 이상의 값을 반환하는 경우 있을 수 있음.

- ANY : 서브쿼리가 둘 이상의 값을 반환할 때, 여러 개 결과 중 한 가지만 만족해도 되게끔. = SOME
- ALL : 서브쿼리가 둘 이상의 값을 반환할 때, 여러 개의 결과를 모두 만족시켜야 함.
- '=ANY (서브쿼리)' 는 'In (서브쿼리)' 과 동일 의미.



#### 원하는 순서대로 정렬하여 출력 : ORDER BY

결과물에 대해 영향을 미치지는 않지만, 결과가 출력되는 순서를 조절하는 구문.

```
SELECT userName, mDate FROM userTBL ORDER BY mDate;
```

- 기본적으로 오름차순으로 정렬. 내림차순으로 정렬 위해서는 열 이름 뒤에 `DESC` 적어주면 됨.
- ORDER BY절은 SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY 중 제일 뒤에 와야 함.



#### 중복된 것은 하나만 남기는 DISTINCT

중복된 것을 제외하고 개수를 셀 때 사용.

```
SELECT DISTINCT addr FROM userTBL;
```



#### ROWNUM 열과 SAMPLE문

- ROWNUM : 앞에서 몇 개, 뒤에서 몇 개 뽑아서 확인하고 싶을 때 사용.

```
SELECT * FROM
	(SELECT employee_id, hire_date FROM employees  ORDER BY hire_date ASC)
	WHERE ROWNUM <= 5;
```

- SAMPLE : 퍼센트 반환.



#### 테이블을 복사하는 CREATE TABLE ... AS SELECT

테이블을 복사해서 사용할 경우에 주로 사용.

```
형식:
CREATE TABLE 새로운테이블 AS (SELECT 복사할 열 FROM 기존 테이블)
```

ex)

```
CREATE TABLE butTBL2 AS (SELECT * FROM buyTBL);
SELECT * FROM buy TBL2;
```

- PK나 FK 등의 제약 조건은 복사되지 않음.
- 그냥 테스트용으로 복제해서 사용하는 정도.





 

