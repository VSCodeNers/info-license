## 정보처리기능사 실기

- SQL이란?  
    - 관계 데이터베이스에서 사용되는 대표적인 언어  

- SQL 정의어(DDL, Data Definition Language)  
    - 관게 데이터베이스에서 사용될 테이블, 스키마, 도메인, 인덱스, 뷰 등을 정의(생성)하거나 수정/제거하기 위해 사용되는 언어  
    - CREATE문, ALTER문, DROP문 등  

    (1) DDL 대상  
        - 스키마(Schema) : DBMS 특성과 구현 환경을 감안한 데이터 구조  
        - 도메인(Domain) : 속성의 데이터 타입과 크기, 제약조건 등을 지정한 정보  
        - 테이블(Table) : 데이터 저장 공간  
        - 뷰(View) : 하나 이상의 물리 테이블에서 유도되는 가상의 논리 테이블  
        - 인덱스(Index) : 검색을 빠르게 하기 위한 데이터 구조  

    (2) DDL 유형  
        - CREATE : 데이터베이스 오브젝트 생성  
            - 테이블 정의  
                - CREATE TABLE 문에 의해 생성  
                구문 : CREATE TABLE 테이블_이름  
            - 스키마 정의  
                - 시스템 관리자가 일반 사용자에게 스키마 권한을 주기 위한 스키마를 만들기 위해 사용됨  
                - CREATE SCHEMA 문에 의해 생성  
                구문 : CREATE SCHEMA 스키마_이름 AUTHORIZATION 사용자;  
            - 도메인 정의  
                - CREATE DOMAIN 문에 의해 생성  
                구문 : CREATE DOMAIN 도메인_이름 데이터_타입  
            - 인덱스 정의  
                - 데이터베이스 내의 자료를 효율적으로 검색하기 위해 인덱스를 만듦  
                - CREATE INDEX 문에 의해 생성  
                구문 : CREATE INDEX 인덱스_이름  

        - ALTER : 데이터베이스 오브젝트 변경  
            - 구문  
                ALTER TABLE 테이블_이름 ADD 속성_이름 데이터_타입 [DEFAULT];   
                ALTER TABLE 테이블_이름 ALTER 속성_이름 데이터_타입 [SET DEFAULT];  
                ALTER TABLE 테이블_이름 DROP 속성_이름 데이터_타입 [CASCADE | RESTRICT];  
                
                - ALTER TABLE ~ ADD : 기존 테이블에 새로운 속성(항목)을 추가  
                - ALTER TABLE ~ ALTER : 기존 테이블의 속성(항목)에 대한 사항을 변경  
                - ALTER TABLE ~ DROP : 기존 테이블에서 속성(항목)을 제거  

        - DROP : 데이터베이스 오브젝트 삭제 (테이블 전체 삭제)  
            - 구문 
                DROP TABLE 테이블_이름 [CASCADE | RESTRICT];  
                DROP SCHEMA 스키마_이름 [CASCADE | RESTRICT];  
                DROP DOMAIN 도메인_이름 [CASCADE | RESTRICT];  
                DROP VIEW 뷰_이름 [CASCADE | RESTRICT];  
                DROP INDEX 인덱스_이름;  
                DROP CONSTRAINT 제약조건_이름;  
        - TRUNCATE : 데이터베이스 오브젝트 내용 삭제 (테이블 구조는 유지됨)  

- 제약조건 적용  
    - ALTER TABLE을 사용하여 테이블에 제약조건을 추가, 삭제는 가능하나 수정은 불가함  

    (1) 제약조건 유형  
        - PRIMARY KEY  
            - 테이블의 기본키를 정의함  
            - NOT NULL, UNQUE 제약 포함  
        - FOREIGN KEY  
            - 외래키를 정의함  
            - 참조 대상을 테이블 이름(=열 이름)으로 명시해야 하며, 참조 무결성 위배 상황 발생 시 옵션 지정 가능  
            (NOT ACTION, SET DEFAULT, SET NULL, CASADE, RESETRICT)  
        - UNIQUE  
            - 테이블 내에서 동일한 값을 가져서는 안되는 항목에 지정함   
        - NOT NULL  
            - 필수 입력 항목에 대해 제약조건으로 설정함  
        - CHECK   
            - 개발자가 정하는 제약 조건  
            - 상황에 따라 다양한 조건 설정 가능  
