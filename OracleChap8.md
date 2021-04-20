## 2021. 04. 20.



### 7.3.2 CASE

- IF, ELSIF, END IF 대신해서 CASE, WHEN, ELSE, END CASE 사용가능



# 8. 테이블과 뷰

데이터베이스의 핵심개체인테이블에 대해 살펴보고, 가상의 테이블인 뷰에 대해 알아본다.



## 8.1 테이블

- DROP TABLE 테이블명 : 테이블 삭제

- ALTER TABLE 테이블명 : 테이블 수정

- not null + unique = primary key (기본키)

- 외래 키 : 마지막 열의 뒤에 콤마(,)를 입력한 후 관련 문장을 써줘야 한다. (다른 표기법 존재)

  ```
  , FOREIGN KEY(userID) REFERENCES userTBL(userID)
  ```



### 8.1.2 제약조건

- 데이터의 무결성을 지키기 위해 제한된 조건을 의미

- 특정 데이터를 입력할 때 무조건적으로 입력되는 것이 아닌, 어떠한 조건을 만족했을 때 입력되도록 제약할 수 있다.

  - PRIMARY KEY 제약 조건
  - FOREIGN KEY 제약 조건
  - UNIQUE 제약 조건
  - CHECK 제약 조건
  - DEFAULT 정의
  - NULL 값 허용 = NOT NULL 여부를 확인한다.(NOT NULL이 기본)

  

#### UNIQUE 제약조건

- PRIMARY KEY와 거의 비슷하지만 UNIQUE는 NULL값을 허용



#### CHECK 제약 조건

- ex)

  ```
  ALTER TABLE userTbl
  	ADD CONSTRAINT CK_height
  	CHECK  (height >= 0) ;
  ```



#### NULL 값 허용

- PRIMARY KEY가 설정된 열에는 NULL값이 있을 수 없으므로, 생략하면 자동으로 NOT NULL로 인식.
- 아무것도 없는 문자(")를 입력할 경우  -> NULL
- `space` 를 누른 공백 (' ')이나 0 값 -> NOT NULL



### 8.1.4 테이블 삭제

- DROP TABLE



### 8.1.5 테이블 수정

- ALTER TABLE : 아래 내용들 하기 이전에 먼저 선언해 줘야함.



#### 열의 추가

- ADD 열이름 데이터형식(데이터길이) 제약조건



#### 열의 삭제

- 제약 조건이 걸린 열을 살제할 경우에는 제약 조건을 먼저 삭제한 후에 열을 삭제해야 함.



#### 열 이름 변경

-  RENAME COLUMN 기존열이름 TO 바꿀열이름



#### 열의 데이터 형식 변경

- MODIFY (바꿀열 데이터형식(크기) NULL유무)



#### 열의 제약 조건 추가 및 삭제

- DROP 제약조건





### 8.2.1 뷰의 개념

- SELECT로 열 몇 개를 가져와서 출력한 결과는 결국 테이블의 모양을 가지고 있음.

- 그렇게 출력된 결과를 3개의 열을 가진 테이블로 봐도 무방 -> 이것이 뷰의 개념.

- 뷰를 생성하는 구문

  ```
  CREATE VIEW v_userTBL
  AS
  	SELECT userID, userName, addr FROM userTBL;
  ```

  ```
  SELECT * FROM v_userTBL;
  ```

  뷰를 테이블이라고 생각해도 무방



### 8.2.2 뷰의 장점

- 보안에 도움이 된다
- 복잡한 쿼리를 단순화 시켜줄 수 있다.



### 8.2.3 구체화된 뷰

- 실제 데이터가 존재하는 뷰
- 조회할 때마다 많은 계산 비용이 드는 집계함수나 여러 테이블을 조인하는 결과 집합에 적합.