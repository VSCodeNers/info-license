## 01.[SQL 활용](https://github.com/VSCodeNers/info-license/tree/main/01.SQL%ED%99%9C%EC%9A%A9) | 01.[기본 SQL](https://github.com/VSCodeNers/info-license/tree/main/01.SQL%ED%99%9C%EC%9A%A9/01.%EA%B8%B0%EB%B3%B8SQL) | 01.DDL(SQL 정의어)
Date: 2023.09.18.월 ~ 2023.09.24.일  

---

- SQL: 관계 데이터베이스에서 사용되는 대표적인 언어
- SQL 정의어(DDL: Data Definition Language)
  - 관계 데이터베이스에서 사용될 오브젝트를 정의(생성)하거나 수정, 제거하는 데 사용되는 언어
  - 오브젝트(DDL 대상)의 유형
    - 스키마(Schema): DBMS의 특성과 구현 환경을 감안한 데이터 구조 (=하나의 데이터베이스)
    - 도메인(Domain): 속성의 데이터 타입과 트기, 제약조건 등을 지정한 정보
    - 테이블(Table): 데이터 저장 공간
    - 뷰(View): 가상의 논리 테이블
    - 인덱스(Index): 검색을 빠르게 하기 위한 데이터 구조
  - DDL 유형
    - CREATE: 데이터베이스 오브젝트 생성
    - ALTER: 데이터베이스 오브젝트 변경
    - DROP: 데이터베이스 오브젝트 삭제
    - TRUNCATE: 데이터베이스 오브젝트 내용 삭제, 테이블 구조는 유지

---

### 1) 생성: CREATE
- 테이블 정의
```
CREATE TABLE 테이블_이름 (
  {속성_이름 데이터_타입 [DEFAULT 값] [NOT NULL],}
  [PRIMARY KEY(속성_이름),]
  [FOREIGN KEY(속성_이름) REFERENCES 참조테이블(속성_이름)]
    [ON DELETE CASCADE | SET NULL | SET DEFAULT | NO ACTION]
    [ON UPDATE CASCADE | SET NULL | SET DEFAULT | NO ACTION],
  [UNIQUE(속성_이름),]
  [CONSTRAINT 제약조건_이름 CHECK(속성_이름=범위 값)]
```
- 다른 테이블 정보를 이용해 생성
```
CREATE TABLE 테이블_이름 AS SELECT문;
```

- [x] {}: 반복, []: 생략 가능, |: 선택
- [x] 속성_이름 데이터_타입 [DEFAULT 값] [NOT NULL]: 테이블을 구성할 속성 이름과 데이터 타입 기입, 속성 수만큼 반복
- [x] PRIMARY KEY(속성_이름): 기본키 속성 지정
- [x] FOREIGN KEY(속성_이름): 외래키 지정
- [x] UNIQUE(속성_이름): 대체키 지정. 중복 값이 없도록 함. (모든 속성값이 교유한 값을 가짐)
- [x] CONSTRAINT 제약조건_이름 CHECK(속성_이름=범위 값): 속성값의 범위 지정
<br/>

- 스키마 정의: 시스템 관리자가 일반 사용자에게 스키마 권한을 주기 위해
```
CREATE SCHEMA 스키마_이름 AUTHORIZATION 사용자;
```
<br/>

- 도메인 정의
```
CREATE DOMAIN 도메인_이름 데이터_타입
  [DEFAULT 기본값]
  [CONSTRAINT 제약조건_이름 CHECK(VALUE IN(범위 값))];
```
<br/>

- 인덱스 정의: 자료를 효율적으로 검색하기 위해 생성. 시스템에 의해 자동 관리
```
CREATE [UNIQUE] INDEX 인덱스_이름
  ON 테이블_이름(속성_이름 [ASC | DESC])
  [CLUSTER];
```
- [x] UNIQUE: 중복을 허용하지 않음
- [x] ON 테이블_이름(속성_이름): 지정된 테이블의 속성으로 인덱스 정의
- [x] ASC | DESC: 정렬 방법
- [x] CLUSTER: 인접된 튜플들을 물리적인 그룹으로 묶어 저장
<br/>

### 2) 변경: ALTER
- 속성 추가
```
ALTER TABLE 테이블_이름 ADD 속성_이름 데이터_타입 [DEFAULT];
```
- 속성 데이터 타입 변경
```
ALTER TABLE 테이블_이름 MODIFY 속성_이름 데이터_타입 [DEFAULT];
```
- 속성 삭제
```
ALTER TABLE 테이블_이름 DROP 속성_이름 [CASCADE | RESTRICT];
```
- 테이블 이름 변경
```
ALTER TABLE 이전_테이블명 RENAME 새로운_테이블명;
```
```
RENAME TABLE 이전_테이블명 TO 새로운_테이블명;
```

### 3) 삭제: DROP, TRUNCATE
- 테이블 삭제
```
DROP TABLE 테이블_이름 [CASCADE | RESTRICT];
```
- 스키마 삭제
```
DROP SCHEMA 스키마_이름 [CASCADE | RESTRICT];
```
- 도메인 삭제
```
DROP DOMAIN 도메인_이름 [CASCADE | RESTRICT];
```
- 뷰 삭제
```
DROP VIEW 뷰_이름 [CASCADE | RESTRICT];
```
- 인덱스 삭제
```
DROP INDEX 인덱스_이름;
```
- 제약조건 삭제
```
DROP CONSTRAINT 제약조건_이름;
```
- 테이블 내용 삭제
```
TRUNCATE TABLE 테이블_이름;
```
<br/>

- [x] RESTRICT: 삭제할 요소가 사용(참조) 중이면 삭제가 이루어지지 않음
- [x] CASCADE: 삭제할 요소가 사용(참조) 중이면, 삭제할 테이블을 참조중엔 다른 테이블에서도 연쇄적으로 같이 삭제됨
<br/>

### 4) 데이터 타입
- CHAR(문자수): 고정길이 문자열
- VARCHAR(문자수): 가변길이 문자열
- INT: 정수
- FLOAT: 실수
- TIME: 시간
- DATE: 날짜

### 5) 제약조건
- 제약조건 유형
  - PRIMARY KEY
    - 테이블의 기본키를 정의
    - 기본으로 NOT NULL, UNIQUE 제약 포함
  - FOREIGN KEY
    - 외래키 정의
    - 참조 대상을 테이블_이름(속성_이름)으로 명시
    - 참조 무결성 위배 상활 발생 시, 처리 방법으로 옵션 지정  
      : NO ACTION, SET DEFAULT, SET NULL, CASCADE, RESTRICT
  - UNIQUE
    - 해당 속성은 유일한 값을 가짐
    - 동일한 값을 가지면 안 되는 항목에 지정
  - NOT NULL
    - 테이블 내에서 관련 속성 값은 NULL일 수 없음
    - 필수 입력 항목
  - CHECK
    - 개발자가 정의하는 제약조건
    - 상황에 따라 다양한 조건 설정 가능
<br/>

- 제약조건 변경
  - 제약조건 추가
    ```
    ALTER TABLE 테이블_이름
    ADD [CONSTRAINT 제약조건_이름] 제약조건(속성_이름);
    ```
  - 제약조건 삭제
    ```
    ALTER TABLE 테이블_이름
    DROP FOREIGN KEY [제약조건_이름];
    ```
  - 제약조건 활성화
    ```
    ALTER TABLE 테이블_이름
    ENABLE CONSTRAINT 제약조건_이름;
    ```
  - 제약조건 비활성화
    ```
    ALTER TABLE 테이블_이름
    DISABLE CONSTRAINT 제약조건_이름;
    ```
<br/>

---

### 연습 문제
**1. 다음 괄호에 들어갈 알맞은 내용을 채우시오.**  
  - [x] 관계 데이터베이스에서 데이터베이스에 필요한 내용을 정의/관리하거나 원하는 결과를 얻기 위해 관계 대수와 관계 해석을 기초로 데이터베이스의 작업을 보다 효율적이고, 다양하게 표현하고 처리하기 위해 사용되는 대표적인 관계 데이터베이스 언어를 말하며, 대화식 언어이다.  
  ✅ SQL  
  - [x] 관계 데이터베이스에서 사용되는 테이블, 스키마, 도메인, 인덱스, 뷰 등을 정의하거나 수정/제거하기 위해 사용되는 언어를 말하며, 종류로는 CREATE 문, ALTER 문, DROP 문이 있다.  
  ✅ 정의어(DDL)  
  - [x] (✅ CREATE) 명령어는 관계 데이터베이스에 필요한 테이블, 스키마, 도메인, 인덱스, 뷰 등을 정의하기 위해 사용되는 명령문을 말한다. 테이블의 정의는 '(✅ CREATE) TABLE 테이블_명' 명령어 구문을 이용한다.
  - [x] 관계 데이터베이스에서 이미 만들어진 기존의 테이블에 새로운 항목을 추가하거나 변경 또는 항목의 삭제등에 사용하는 명령어를 말한다.  
  (✅ ALTER) 명령의 기본 사용법은 다음과 같다.  
    - 항목 추가: (✅ ALTER) TABLE ~ ADD
    - 항목 사항 변경: (✅ ALTER) TABLE ~ MODIFY
    - 항목 삭제: (✅ ALTER) TABLE ~ DROP
  - [x] DROP 명령문은 관계 데이터베이스에서 사용되던 테이블, 스키마, 도메인, 인덱스, 뷰, 제약조건 등을 제거할 때 사용되는 명령을 말한다.  
  DROP 명령에 사용되는 옵션 (✅ CASCADE)와 RESTICT가 있으며 RESTICT는 삭제할 요소가 참조 중이면 삭제되지 않도록 하는 옵션을 말하고, (✅ CASCADE)는 삭제할 요소가 참조 중이더라도 삭제가 이루어지며, 삭제할 테이블을 참조 중인 다른 테이블도 연쇄적으로 같이 삭제가 되도록 하는 옵션을 말한다.
<br/>

**2. 데이터 정의 언어(DDL)에서 테이블 구조는 유지하며 테이블 내용을 제거하는 명령어를 작성하시오.**  
✅ TRUNCATE  
<br/>

**3. 아래 보기는 '영진'테이블의 기본키를 삭제하는 SQL문이다. 빈칸에 들어갈 알맞은 SQL을 모두 작성하시오.**  
```
( 1 ) TABLE 영진 ( 2 ) PRIMARY KEY;
```
✅ 1: ALTER, 2: DROP
