---
description: 편의점 재고관리를 연습했습니다.
---

# 편의점 재고관리

* /🗓️프로젝트 기간\[23.10.18\~23.10.26]

### 프로젝트 개요

<details>

<summary>📌개발 목적</summary>

편의점 데이터 베이스를 구축하여 효율적인 재고관리를 하기 위해 진행하였습니다.

</details>

<details>

<summary>🛠활용 도구</summary>

<img src="https://img.shields.io/badge/oracle-F80000?style=for-the-badge&#x26;logo=oracle&logoColor=white">

</details>

<details>

<summary>⚙️구성</summary>

![image](https://github.com/jks92-bb/introduction/assets/49471248/587b36c8-1632-486d-bcb1-3c63d8d65400)


</details>

<details>

<summary>📃중점 코드</summary>

결과물을 얻기위한 메인 sql문입니다.
## MAIN
```sql
-- 트리거 사용하기위해 서버 아웃풋을 ON 시킨다
SET SERVEROUTPUT ON;
-- 상품코드 846543부터 시작해서 17씩 증가
CREATE SEQUENCE FOOD_BARCODE START WITH 846543 INCREMENT BY 17; 
 -- 인벤토리코드 846543부터 시작해서 17씩 증가
CREATE SEQUENCE INVENTORY_BARCODE START WITH 846543 INCREMENT BY 17;

CREATE TABLE CATEGORY(                                  -- 품목 테이블 (이 테이블의 데이터값은 고정)
    CODE VARCHAR2(10) PRIMARY KEY,                      -- 품목분류코드 
    KIND VARCHAR2(20)                                   -- 품목 분류
);
CREATE TABLE COMPANY(                                   -- 회사 테이블
    CODE INT DEFAULT FOOD_BARCODE.NEXTVAL PRIMARY KEY,  -- 상품 코드(상품마다 다름, 시퀀스 사용)
    COMPANY VARCHAR2(30)                                -- 제조회사명
);
CREATE TABLE INVENTORY(                                 -- 관리 테이블 
    CATEGORY_CODE VARCHAR2(10),                         -- 품목분류하기위한 코드 (외래키) -- CATEGORY 테이블의 CODE랑 연결
    FOOD_CODE INT DEFAULT INVENTORY_BARCODE.NEXTVAL,    -- 상품 코드(외래키) -- COMPANY 테이블의 CODE랑 연결
    NAME VARCHAR2(50),                                  -- 상품명 
    CNT INT ,                                           -- 재고량
    PRICE INT                                           -- 음식 가격
);
-- CATEGORY 테이블의 CODE를 참조하여 INVENTORY 테이블의 CATEGORY_CODE를 외래키로 지정
ALTER TABLE INVENTORY ADD CONSTRAINT FK_CATEGORY_CODE FOREIGN KEY(CATEGORY_CODE) REFERENCES CATEGORY(CODE);                                                                                                              
 -- COMPANY 테이블의 CODE를 참조하여 INVENTORY 테이블의 FOOD_CODE를 외래키로 지정                                                                                      
ALTER TABLE INVENTORY ADD CONSTRAINT FK_FOOD_CODE FOREIGN KEY(FOOD_CODE) REFERENCES COMPANY(CODE);     
                                                                                       
-------------------------------------조회-------------------------------------
-- 모든 테이블 JOIN해서 모두 조회
SELECT * FROM INVENTORY JOIN COMPANY ON INVENTORY.FOOD_CODE = COMPANY.CODE JOIN CATEGORY ON CATEGORY.CODE = INVENTORY.CATEGORY_CODE; 
-- 모든 테이블 조회 하면 겹치는 속성값(상품코드,카테고리코드)도 나오니까 필요한 정보들만 속성 뽑아서 뷰 만들기.
CREATE VIEW ALL_INFO AS
SELECT KIND, NAME, PRICE, CNT, COMPANY, FOOD_CODE 음식코드 FROM COMPANY, CATEGORY, INVENTORY WHERE INVENTORY.FOOD_CODE = COMPANY.CODE AND CATEGORY.CODE = INVENTORY.CATEGORY_CODE;
-- 만들어진 뷰 조회(뷰 이름 : ALL_INFO)
SELECT * FROM ALL_INFO;
-- 회사별 상품 종류
SELECT COMPANY, COUNT(*) 상품종류 FROM ALL_INFO GROUP BY COMPANY;
-- 회사별 상품이 3개 종류 이상 출력
SELECT COMPANY, COUNT(*) 상품종류 FROM ALL_INFO GROUP BY COMPANY HAVING COUNT(*) >= 3;
-- 특정 이름을 검색 후(EX.초코) 음식코드 ,종류 ,음식명 ,가격 ,재고량 ,제조사 출력하기
SELECT * FROM ALL_INFO WHERE NAME LIKE '%초코%'; 
-- 제일 낮은 가격의 상품 정보를 출력하기 (중첩질의문 사용)
SELECT * FROM ALL_INFO WHERE PRICE = (SELECT MIN(PRICE) FROM ALL_INFO);
-------------------------- 수정-----------------------------------------
-- COMPANY 테이블에 데이터 추가하는 프로시저 호출
EXEC INSERT_COMPANY(FOOD_BARCODE.NEXTVAL,'테스트회사');
-- 확인 출력
SELECT * FROM COMPANY;
-- INVENTORY 테이블에 데이터 추가하는 프로시저 호출 
EXEC INSERT_INVENTORY('테스트까까','AB01',INVENTORY_BARCODE.NEXTVAL,1,3000);
-- 확인 출력
SELECT * FROM INVENTORY;
-- 상품의 이름이 조건에 맞을때에 재고량을 수정하는 프로시저 호출 -> 매개변수(수정할 재고량, 수정할 제품명)
EXEC UPDATE_CNT(2,'후라이드치킨'); 
-- 재고 확인 출력
SELECT NAME,CNT FROM INVENTORY;
-- 상품의 이름이 조건에 맞을때에 가격을 수정하는 프로시저 호출 -> 매개변수(수정할 가격, 수정할 제품명)
EXEC UPDATE_PRICE(9000,'후라이드치킨');
-- 재고 확인 출력
SELECT NAME,PRICE FROM INVENTORY;
---------------------- 삭제 ------------------------------------
-- 전체데이터(INVENTORY) 삭제 프로시저 호출
EXEC DELETE_ALL();
-- INVENTORY 확인 출력
SELECT * FROM INVENTORY;
SELECT * FROM COMPANY;
-- 바코드 입력 받아서 삭제 프로시저 호출
EXEC DELETE_INVENTORY_CODE(846543);
EXEC DELETE_COMPANY_CODE(846543);
-- INVENTORY 확인 출력
SELECT * FROM COMPANY;
SELECT * FROM INVENTORY;

COMMIT;

```
삭제 프로시저를 따로 두었습니다.
## DELETE_PROCEDURE
```sql
-- 1. 전체 데이터 삭제
CREATE OR REPLACE PROCEDURE DELETE_ALL
IS BEGIN
    DELETE FROM INVENTORY;
    DELETE FROM COMPANY;
END DELETE_ALL;
/
-- 2. 제품 데이터삭제 - 바코드로
CREATE OR REPLACE PROCEDURE DELETE_INVENTORY_CODE(
    DEL_FOOD_CODE IN INT
)
IS BEGIN
    DELETE FROM INVENTORY WHERE FOOD_CODE = DEL_FOOD_CODE;
END DELETE_INVENTORY_CODE;
/
CREATE OR REPLACE PROCEDURE DELETE_COMPANY_CODE(
    DEL_COMPANY_CODE IN INT
)
IS BEGIN
    DELETE FROM COMPANY WHERE CODE = DEL_COMPANY_CODE;
END DELETE_COMPANY_CODE;
```

데이터의 변경시 확인 할 수 있는 알림을 주기위해 트리거를 작성하였습니다.
## TRIGGER
```sql
-- 1. 각각 추가될때 '데이터 추가되었습니다.'
-- COMPANY 테이블 추가
CREATE OR REPLACE TRIGGER ALARM_INSERT_COMPANY
BEFORE INSERT ON COMPANY     
FOR EACH ROW
DECLARE BEGIN
    DBMS_OUTPUT.PUT_LINE('데이터가 추가되었습니다.');
END; 
/
-- IVENTORY 테이블 추가
CREATE OR REPLACE TRIGGER ALARM_INSERT_INVENTORY
BEFORE INSERT ON INVENTORY    
FOR EACH ROW
DECLARE BEGIN
    DBMS_OUTPUT.PUT_LINE('데이터가 추가되었습니다.');
END; 
/
-- 2. 각각 수정될 '데이터 수정되었습니다.'
-- COMPANY 테이블 수정
CREATE OR REPLACE TRIGGER ALARM_UPDATE_COMPANY
BEFORE UPDATE ON COMPANY     
FOR EACH ROW
DECLARE BEGIN
    DBMS_OUTPUT.PUT_LINE('데이터가 수정되었습니다.');
END; 
/
-- IVENTORY 테이블 수정
CREATE OR REPLACE TRIGGER ALARM_UPDATE_INVENTORY
BEFORE UPDATE ON INVENTORY    
FOR EACH ROW
DECLARE BEGIN
    DBMS_OUTPUT.PUT_LINE('데이터가 수정되었습니다.');
END; 
/
-- 3. 각각 삭제될때 '데이터 삭제되었습니다.'
-- COMPANY 테이블 삭제
CREATE OR REPLACE TRIGGER ALARM_DELETE_COMPANY
BEFORE DELETE ON COMPANY     
FOR EACH ROW
DECLARE BEGIN
    DBMS_OUTPUT.PUT_LINE('데이터가 삭제되었습니다.');
END; 
/
-- IVENTORY 테이블 삭제
CREATE OR REPLACE TRIGGER ALARM_DELETE_INVENTORY
BEFORE DELETE ON INVENTORY    
FOR EACH ROW
DECLARE BEGIN
    DBMS_OUTPUT.PUT_LINE('데이터가 삭제되었습니다.');
END; 
/
```




데이터를 원래 대용량 받아와서 작업을 진행하려 했으나 수작업으로 찾아서 기입하였습니다.
## INVENTORY_DATA
```sql
-- 관리 테이블 데이터 추가/ 상품이름(NAME), 품목 코드(FOOD_CODE), 재고(CNT), 가격(PRICE), 
-- 간편식사 데이터 입력
INSERT INTO INVENTORY(NAME,CATEGORY_CODE,CNT,PRICE) VALUES ('후라이드치킨' ,'AB01', 3, 9900); 
INSERT INTO INVENTORY(NAME,CATEGORY_CODE,CNT,PRICE) VALUES ('소시지바' ,'AB01', 2, 2400); 
INSERT INTO INVENTORY(NAME,CATEGORY_CODE,CNT,PRICE) VALUES ('브라우니쿠키' ,'AB01', 10, 1000); 
INSERT INTO INVENTORY(NAME,CATEGORY_CODE,CNT,PRICE) VALUES ('글레이즈드도넛' ,'AB01', 15, 1300); 
INSERT INTO INVENTORY(NAME,CATEGORY_CODE,CNT,PRICE) VALUES ('뉴자이언트지파이' ,'AB01', 4, 2900); 
INSERT INTO INVENTORY(NAME,CATEGORY_CODE,CNT,PRICE) VALUES ('크로와상' ,'AB01', 10, 1300);
```


</details>

<details>

<summary>🔎프로젝트 git 주소</summary>



</details>
