## 2021. 04. 20.



# 9. 인덱스



## 9.1 인덱스의 개념

- 데이터를 좀 더 빠르게 찾을 수 있도록 해주는 도구
- 필요 없는 인덱스를 만들어 데이터베이스가 차지하는 공간만 더 늘어나는 경우도 있음
- 튜닝에 즉각적인 효과를 내는 가장 빠른 방법 중 하나.
- 인덱스의 장점
  - ㅇㅇ
- 인덱스의 단점
  - ㅇㅇ



## 9.2 인덱스의 종류와 자동 생성



### 9.2.1 Oracle에서 사용되는 인덱스의 종류

- B-TREE 인덱스
- BITMAP 인덱스
- 함수기반 인덱스
- 어플리케이션 도메인 인덱스



### 9.2.2 자동으로 생성되는 인덱스

- 인덱스는 테이블의 열 단위에 생성됨
- 테이블 생성 시에 제약 조건 Primary Key 또는 Unique를 사용하면 자동으로 인덱스가 자동 생성된다.



## 9.6 결론 : 인덱스를 생성해야 하는 경우와 그렇지 않은 경우

- 인덱스는 열 단위에 생성
- WHERE절에서 사용되는 열에 인덱스를 만들어야 함
- WHERE절에 사용되더라도 자주 사용해야 가치가 있다
- 데이터의 중복도가 높은 열은 인덱스를 만들어도 별 효과가 없다.
- JOIN에 자주 사용되는 열에는 인덱스를 생성해 주는 것이 좋다
- INSERT/UPDATE/DELETE가 얼마나 자주 일어나는 지를 고려해야 한다
- 사용하지 않는 인덱스는 제거하자



