## 1. 테이블 생성 DDL

```
CREATE TABLE MAIN_LIST (
ID         VARCHAR(30)      NOT NULL
,LV        VARCHAR(10)      NOT NULL
,PARENT_ID VARCHAR(30)     
,NAME      VARCHAR(100) 
);

CREATE TABLE CHECK_LIST (
ID         VARCHAR(30)      NOT NULL
,MAIN_ID VARCHAR(30)        NOT NULL
,NAME      VARCHAR(100) 
,ISCHECK VARCHAR(1)
);
```

## 2. 데이터입력 DML
```
INSERT INTO MAIN_LIST VALUES('M001','1',TRIM('    '),TRIM('1메인점검  '));
INSERT INTO MAIN_LIST VALUES('M002','2',TRIM('M001'),TRIM('1-1단계하위'));
INSERT INTO MAIN_LIST VALUES('M003','3',TRIM('M002'),TRIM('1-2단계하위'));
INSERT INTO MAIN_LIST VALUES('M004','4',TRIM('M004'),TRIM('1-3단계하위'));
INSERT INTO MAIN_LIST VALUES('M005','1',TRIM('    '),TRIM('2메인점검  '));
INSERT INTO MAIN_LIST VALUES('M006','2',TRIM('M005'),TRIM('2-1단계하위'));
INSERT INTO MAIN_LIST VALUES('M007','3',TRIM('M006'),TRIM('2-2단계하위'));
INSERT INTO MAIN_LIST VALUES('M008','4',TRIM('M007'),TRIM('2-3단계하위'));
INSERT INTO MAIN_LIST VALUES('M009','5',TRIM('M008'),TRIM('2-4단계하위'));
INSERT INTO MAIN_LIST VALUES('M010','2',TRIM('M005'),TRIM('2-5단계하위'));
                                      
INSERT INTO CHECK_LIST VALUES('C001','M001',TRIM('1메인체크      '),'Y');
INSERT INTO CHECK_LIST VALUES('C002','M002',TRIM('1-1단계하위체크'),'Y');                
INSERT INTO CHECK_LIST VALUES('C003','M003',TRIM('1-2단계하위체크'),'Y');
INSERT INTO CHECK_LIST VALUES('C004','M004',TRIM('1-3단계하위체크'),'Y');
INSERT INTO CHECK_LIST VALUES('C005','M005',TRIM('2메인점검체크  '),'Y');
INSERT INTO CHECK_LIST VALUES('C006','M006',TRIM('2-1단계하위체크'),'Y');
INSERT INTO CHECK_LIST VALUES('C007','M007',TRIM('2-2단계하위체크'),'Y');
INSERT INTO CHECK_LIST VALUES('C008','M008',TRIM('2-3단계하위체크'),'Y');
INSERT INTO CHECK_LIST VALUES('C009','M008',TRIM('2-4단계하위체크'),'Y');
INSERT INTO CHECK_LIST VALUES('C010','M009',TRIM('2-5단계하위체크'),'Y');
```

## 3. 조회문
```
SELECT M.ID
       ,M.LV
       ,M.PARENT_ID
       ,M.NAME
       ,C.ID
       ,C.MAIN_ID
       ,C.NAME CHECK_NAME
       ,C.ISCHECK
      ,SYS_CONNECT_BY_PATH(C.ISCHECK,',') 상위부터순차적Y여부
      ,CASE WHEN SYS_CONNECT_BY_PATH(C.ISCHECK,',') LIKE '%N%' THEN 'N' ELSE 'Y' END 최종여부
FROM MAIN_LIST M
    ,CHECK_LIST C
WHERE M.ID = C.MAIN_ID(+)
START WITH M.LV = '1'
CONNECT BY PRIOR M.ID = M.PARENT_ID;
```

## 4. 결과
![[/2.이미지/트리검색.png]]