## 2021. 04. 19.



# 7. PL/SQL 고급

- SQL문의 데이터 형식및 변수, 대용량 데이터의 저장에 대해 학습
- CLOB : Character Large Object



## 7.1 Oracle의 데이터 형식



### 7.1.1 Oracle에서 지원하는 데이터 형식의 종류

- 숫자 데이터 형식
  - NUMBER(p,[s]) : 전체 자릿수(p)와 소수점 이하 자릿수(s)를 가진 숫자형. setInt, setFloat와 호환
- 문자 데이터 형식
  - CHAR[(n)]
  - NCHAR[(n)]
  - VARCHAR2(n) : 가변길이 문자형. 가장 많이 사용. setString 과 호환
  - NVARCHAR2(n)
  - CLOB
  - NCLOB
- 이진 데이터 형삭
  - BLOB : 대용량 이진 데이터를 저장할 수 있는 데이터 타입.
- 널짜와 시간 데이터 형식
  - DATE : 연, 월, 일, 시, 분, 초 저장  setDate와 호환.
  - TIMESTAMP : Date와 같으나 밀리초 단위까지 저장됨.

- 기타 데이터 형식
  - RAWID : 행의 물리적인 주소를 저장하기 위한 데이터 형식으로 모든 행에 자동으로 RAWID 열이 생성됨. ROW IDentity의 약자
  - XMLType : XML 데이터를 저장하기 위한 데이터 형식
  - URIType : URL 형식의 데이터를 저장하기 위한 데이터 형식





## 7.2 조인

- 두 개 이상의 테이블을 서로 묶어서 하나의 결과 집합으로 만들어 내는 것.



### 7.2.1 INNER JOIN(내부 조인)

- 조인 중에서 가장 많이 사용되는 조인.

  ```
  SELECT <열 목록>
  FROM <첫 번째 테이블>
  	INNER JOIN <두 번째 테이블>
  	ON <조인될 조건>
  [WHERE 검색조건];
  ```

- 각 테이블에 별칭(Alias)을 줘서 코드를 간결하게 만들 수 있음.

- ORDER BY로 정렬 가능 - ON으로 JOIN한 이후에 써야함.

- 세 개 테이블의 조인을 하는 방법 : 다대다의 관계일 때 두 테이블의 사이에 연결 테이블을 둬서 이 연결 테이블과 두 테이블이 일대다 관계를 맺도록 구성.



### 7.2.2 OUTER JOIN(외부 조인)

- 조인의 조건에 만족되지 않는 행까지도 포함시키는 것.
  - ex) 부서 배치를 받지 못한 직원, 직원이 없는 부서 등.
- LEFT : 왼쪽 테이블 전부, RIGHT : 오른쪽 테이블 전부, FULL : 왼쪽 오른쪽 테이블 전부



### 7.2.3 CROSS JOIN(상호 조인)

- 한쪽 테이블의 모든 행들과 다른 쪽 테이블의 모든 행을 조인시키는 기능을 함.
- CROSS JOIN의 결과 개수 = 두 테이블 개수를 곱한 개수

- 테스트로 사용할 많은 양의 데이터를 생성할 때 주로 사용.

  - ex) 

    ```
    CREATE TABLE ... AS (SELECT...)
    ```



### 7.2.4 SELF JOIN

- 별도의 구문이 있는 것이 아니라 자기 자신과 자기 자신이 조인한다는 의미. 
- 하나의 테이블에 같은 데이터가 존재하되 의미는 다르게 존재하는 경우에 두 테이블을 서로 SELF JOIN 시켜서 정보 확인 가능.

- ex)

  ```
  SELECT A.emp AS "부하직원", B.emp AS "직속상관", B.department AS "직속상관부서"
  	 FROM empTbl A
  	 	INNER JOIN empTbl B
  	 		ON A.manager = B.emp
  	 WHERE A.emp = "우대리";
  ```

  

### 7.2.5 UNION / UNION ALL / NOT IN / IN

- UNION은 두 쿼리의 결과를 행으로 합치는 것.
- 전처리 작업에서 많이 함.
- NOT IN : 첫 번째 쿼리의 결과 중에서, 두 번째 쿼리에 해당하는 것을 제외하기 위한 구문.