---
layout: post
title:  "SQL 기초 정리"
date:   2016-07-22 01:02:00 +0900
categories: dev
tags: 
- sql
- db
- rdb
- oracle
---


# SQL개발


## SQL기본

- DDL;Data Definition Language : 스키마를 정의하거나 조작하기 위해 사용
	- CREATE : 정의
	- ALTER : 수정
	- DROP : 삭제
	- TRUNCATE : DROP 후 CREATE

- DML;Data Manipulation Language : 데이터를 조작하기 위해 사용
	- SELECT : 조회
	- INSERT : 추가
	- DELETE : 삭제
	- UPDATE : 변경

- DCL;Data Control Language : 데이터를 제어하기 위해 사용
	- COMMIT : 트랜잭션 작업 결과를 반영
	- ROLLBACK : 트랜잭션의 작업 취소 및 원상복구
	- GRANT : 사용자에게 권한 부여
	- REVOKE : 사용자 권한 취소

### DML
- 테이블 생성

~~~
CREATE TABLE 테이블명
(
	컬럼_이름 데이터타입 [컬럼_제약사항]
);
~~~
{: .language-sql}

~~~
CREATE TABLE TBL_EMPLOYEE
(
	EMP_NO NUMBER PRIMARY KEY,
    EMP_NAME VARCHAR2(20),
    EMP_DEPT_ID VARCHAR2(3) REFERENCES TBL_DEPARTMENT
);
~~~
{: .language-sql}

- 테이블 데이터 조회
실행 순서 : FROM => WHERE => ROWNUM(Oracle) => SELECT COLUMN LIST => GROUP BY => HAVING => ORDER BY

~~~
SELECT
컬럼명1,
컬럼명2,
컬럼명3 AS 컬럼별칭,
컬럼명4
FROM 테이블명
[WHERE 컬럼명1 = 조건]
[GROUP BY 컬럼명2]
[HAVING 컬럼명3 > 조건]
[ORDER BY 컬럼명4 ASC|DESC]
;
~~~
{: .language-sql}

~~~
SELECT 
EMP_NO,
EMP_NAME,
EMP_DEPTID AS DEPT_ID,
D.DEPT_NAME AS DEPT_NAME
FROM TBL_EMPLOYEE E, TBL_DEPARTMENT D
WHERE E.EMP_DEPTID = D.DEPT_ID
AND D.DEPT_NAME = '영업팀'
ORDER BY EMP_NO
;
~~~
{: .language-sql}

- 테이블 데이터 입력

~~~
INSERT 테이블명
(컬럼명1, 컬럼명2, ... , 컬럼명N)
VALUES
(데이터1, 데이터2, ... , 데이터N)
;
~~~
{: .language-sql}

~~~
INSERT TBL_EMPLOYEE
(EMP_NO, EMP_NAME, ... , EMP_HIREDATE)
VALUES
(10000, '직원이름, ... , '20170101')
;
~~~
{: .language-sql}

- 테이블 데이터 수정

~~~
UPDATE 테이블명
SET
컬럼명1 = 변경할_데이터1,
컬럼명2 = 변경할_데이터2
WHERE
컬럼명3 = 조건
;
~~~
{: .language-sql}

~~~
UPDATE TBL_EMPLOYEE
SET
EMP_NAME = '포켓몬'
EMP_HIREDATE = '20160101'
WHERE
EMP_NO = 10000
;
~~~
{: .language-sql}

- 테이블 데이터 삭제

~~~
DELETE FROM 테이블명
WHERE
컬럼명1 = 조건
;
~~~
{: .language-sql}

~~~
DELETE FROM TBL_EMPLOYEE
WHERE
EMP_NO = 10000
;
~~~
{: .language-sql}


### TCL
트랜잭션을 제어하는 명령인 `COMMIT`, `ROLLBACK` 만을 따로 분리해서 TCL;Transaction Control Language 라고 표현

- Transaction : 데이터의 일관성을 유지하기 위해 논리적으로 연관된 데이터 변경작업들을 묶어서 관리하는 개념
- 데이터 변경(삽입/수정/삭제)할 때, 변경 내용은 임시 상태로 유지된다.
- 변경한 내용이 DB에 반영되기 위해 `COMMIT` 명령이 필요
- 변경한 내용을 취소하고 원래 상태로 되돌리기 위해서는 `ROLLBACK` 명령이 필요
- 데이터 변경 작업 후 반드시 `COMMIT` 또는 `ROLLBACK` 명령 수행해야 한다.




## 연산자

### 연산자 기본

- BETWEEN
	- 값의 범위를 기반으로 조회

~~~
컬럼명 BETWEEN 범위시작 AND 범위종료 ;
~~~
{: .language-sql}

- 연결연산자 `||`
	- 열이나 문자열을 다른 열에 연결

~~~
SELECT 컬럼명1 || 컬럼명2 AS 합친컬럼ALIAS
FROM 테이블명;
~~~
{: .language-sql}

- LIKE
	- 패턴 일치 검색 수행
	- `%`는 0개 이상의 문자를 나타냄
	- `_`는 한 개 문자를 나타냄

~~~
...
WHERE 컬럼명 LIKE '%ABC_' -- ABC 앞에 0개 이상글자 포함, 뒤에 1개문자 포함
...
~~~
{: .language-sql}

- IN
	- 목록에 있는 값에 대해 조회
	- DB 내부에서 OR조건의 집합으로 해석

~~~
...
WHERE 컬럼명 IN ('A', 'B') -- 컬럼에 'A' 또는 'B'가 있어야 함
...
~~~
{: .language-sql}

- EXISTS
	- 서브쿼리의 결과 집합에 행이 존재하는지 확인

~~~
--MANAGER 역할을 하는 사원정보 확인
SELECT EMP_ID, EMP_NAME
FROM TBL_EMP E
WHERE EXISTS(SELECT 'X' FROM TBL_EMP WHERE E.EMP_ID = MANAGER_ID);
~~~
{: .language-sql}

- NOT EXISTS
	- 서브쿼리의 결과 집합에 행이 존재하지 않는지 확인

~~~
--사원이 없는 부서
SELECT DEPT_NAME
FROM TBL_DEPT D
WHERE NOT EXISTS(SELECT 'X' FROM TBL_EMP WHERE DEPT_ID = D.DEPT_ID);
~~~
{: .language-sql}

### 집합연산자

|연산자 종류|반환 값 |
|---------|--------|
|UNION|중복 행이 제거된 두 쿼리의 행|
|UNION ALL|중복 행이 포함된 두 쿼리의 행|
|INTERSECT|두 쿼리에 공통적인 행|
|MINUS|첫 번째 쿼리에 있는 행 중 두 번째 쿼리에 없는 행|

- UNION ALL의 경우를 제외하면 중복행 제거됨

> 결과 데이터 중복 없고 정렬 필요 없다면, SQL 성능을 위해 UNION대신 UNION ALL 사용 

- 집합연산자 주의사항
	- 컬럼 수 및 대응되는 컬럼의 데이터 유형 동일해야 함
	- ORDER BY 절은 처음 쿼리 기준으로 컬럼명 인식

~~~
SELECT 컬럼1-1, 컬럼1-2
FROM 테이블1
UNION ALL
SELECT 컬럼2-1, 컬럼2-2
FROM 테이블2
ORDER BY 컬럼1-2;
~~~
{: .language-sql}


## 함수

### 문자함수

- LOWER : 알파벳 값을 소문자로 변환

~~~
SELECT LOWER(컬럼명) FROM 테이블명;
-- Name -> name
~~~
{: .language-sql}

- UPPER : 알파벳 값을 대문자로 변환

~~~
SELECT UPPER(컬럼명) FROM 테이블명;
-- Name -> NAME
~~~
{: .language-sql}

- INITCAP : 첫 번째 글자만 대문자로 변환

~~~
SELECT INITCAP(컬럼명) FROM 테이블명;
-- NAME -> Name
~~~
{: .language-sql}

- CONCAT : 두 문자열 연결 (`||` 연결연산자와 같은 기능)

~~~
SELECT CONCAT(컬럼명1, 컬럼명2) FROM 테이블명;
-- Name, Family -> NameFamily
~~~
{: .language-sql}

- SUBSTR : 문자열 중 특정 문자 또는 문자열의 일부분을 선택

~~~
SELECT SUBSTR(컬럼명, 1, 2) FROM 테이블명;
-- Name -> Na
~~~
{: .language-sql}

- LENGTH : 문자열의 길이를 구함

~~~
SELECT LENGTH(컬럼명) FROM 테이블명;
-- Name -> 4
~~~
{: .language-sql}


- INSTR : 명명된 문자의 위치를 구함

~~~
SELECT INSTR(컬럼명, 'a') FROM 테이블명;
-- Name -> 2
~~~
{: .language-sql}

- LPAD : 왼쪽 문자 자리 채움

~~~
SELECT LPAD(컬럼명, 10, '!') FROM 테이블명;
-- Name -> !!!!!!Name
~~~
{: .language-sql}

- RPAD : 오른쪽 문자 자리 채움

~~~
SELECT RPAD(컬럼명, 10, '!') FROM 테이블명;
-- Name -> Name!!!!!!
~~~
{: .language-sql}

- TRIM : 문자열에서 선행문자와 후행문자를 자름

~~~
SELECT TRIM('N' FROM 컬럼명) FROM 테이블명;
-- Name -> ame
~~~
{: .language-sql}

- REPLACE : 특정 문자열을 대치함

~~~
SELECT REPLACE(컬럼명, 'Na', 'Ga') FROM 테이블명;
-- Name -> Game
~~~
{: .language-sql}

### 숫자함수

- ROUND : 지정된 소수점 자릿수로 값을 반올림

~~~
SELECT ROUND(45.87, 1) FROM DUAL;
-- 45.9
~~~
{: .language-sql}

- TRUNC : 지정된 소수점 자릿수로 값을 버림

~~~
SELECT TRUNC(45.87, 1) FROM DUAL;
-- 45.8
~~~
{: .language-sql}

- CEIL : 지정한 값보다 큰 수 중에서 가장 작은 정수

~~~
SELECT CEIL(15.3) FROM DUAL;
-- 16
~~~
{: .language-sql}

- FLOOR : 지정한 값보다 작은 수 중에서 가장 큰 정수

~~~
SELECT FLOOR(15.3) FROM DUAL;
-- 15
~~~
{: .language-sql}

- MOD : 나눈 나머지를 반환

~~~
SELECT MOD(18, 4) FROM DUAL;
-- 2
~~~
{: .language-sql}

### 날짜 함수

- MONTHS_BETWEEN : 두 날짜 간의 월 수 구하기
- ADD_MONTHS : 날짜에 월 추가/감소
- NEXT_DAY : 지정된 날짜의 다음 날
- LAST_DAY : 월의 마지막 날
- ROUND : 날짜 반올림
- TRUNC : 날짜 절삭

### 기타 함수

- NVL(컬럼명, NULL일 경우 치환할 DEFAULT값) : NULL을 대체할 값 

### 데이터 형 변환

- TO_CHAR : DATE타입 / NUMBER 타입을 CHARACTER 타입으로 변환

~~~
TO_CHAR(값, 'FORMAT')
~~~
{: .language-sql}

- TO_DATE : 날짜형식이 아닌 문자열을 DATE타입으로 변환

~~~
TO_DATE(값, 'FORMAT')
~~~
{: .language-sql}

### 그룹함수

- COUNT() : 행의 개수
- SUM() : NULL을 제외한 N의 합계
- MIN() : NULL을 제외한 컬럼의 최소값
- MAX() : NULL을 제외한 컬럼의 최대값
- AVG() : NULL을 제외한 N의 평균값



### Window Function

행과 행간의 관계를 쉽게 정의하기 위해 만든 함수

- WINDOW FUNCTION 종류
	- 순위(RANK) 관련 함수 : RANK, DENSE_RANK, ROW_NUMBER  
	(ANSI/ISO SQL 표준 / Oracle / SQL Server 등 대부분 DBMS 지원)

	- 집계(AGGREGATE) 관련 함수 : SUM, MAX, MIN, AVG, COUNT  
	(ANSI/ISO SQL 표준 / Oracle / SQL Server 등 대부분 DBMS지원)  
    (SQL Server의 경우 집계 함수는 OVER 절 내의 ORDER BY 구문을 지원하지 않는다.)

	- 행 순서 관련 함수 : FIRST_VALUE, LAST_VALUE, LAG, LEAD  
	(Oracle에서만 지원되는 함수)

	- 비율 관련 함수 : CUME_DIST, PERCENT_RANK, NTILE, RATIO_TO_REPORT, RATIO_TO_REPORT  
	(CUME_DIST, PERCENT_RANK : ANSI/ISO SQL 표준/Oracle 지원  
    NTILE : Oracle / SQL Server 지원  
    RATIO_TO_REPORT: Oracle 지원)

	- 선형 분석을 포함한 통계 분석 관련 함수


## SQL활용

### 표준조인

- 조인의 종류

|구분|조인의 종류|내용|
|-|-|-|
|결과집합의 구성유형|Inner Join|결과 집합구성에 필수적인 테이블과 조인<br>일치하는 행만 반환하는 테이블의 조인|
|결과집합의 구성유형|Outer Join|결과 집합구성에 선택적인 테이블과 조인<br>Inner Join의 결과 및 왼쪽(또는 오른쪽)일치하지 않는 행도 반환하는 테이블의 조인|
|조인 연산자의 유형|Equi Join|`=(Equal)` 연산자를 이용한 조인|
|조인 연산자의 유형|Non-Equi Join|`=(Equal)` 이외의 연산자를 이용한 조인 (카테시안 조인 포함)|


> 카테시안 조인(Cartesian Join) : 조인 조건 없거나 일부 누락된 조인으로, Non-Equi Join의 한 유형으로 볼 수 있음.

- ANSI-SQL JOIN

~~~
SELECT 컬럼1[, 컬럼N, ...]
FROM 테이블1
[CROSS JOIN 테이블2] | --카테시안 곱
[NATURAL JOIN 테이블2] |
[JOIN 테이블2 USING (컬럼)] |
[JOIN 테이블2 ON (컬럼1 = 컬럼2)] |
[LEFT|RIGHT|FULL [OUTER] JOIN 테이블2 ON (컬럼1 = 컬럼2) | USING(컬럼)];
~~~
{: .language-sql}

- Oracle-SQL LEFT OUTER JOIN

~~~
SELECT A.EMP_ID, A.EMP_NAME, B.DEPT_NAME
FROM TBL_EMPLOYEE A, TBL_DEPARTMENT B
WHERE A.DEPT_ID = B.DEPT_ID(+);
~~~
{: .language-sql}

### 서브쿼리
하나의 SQL문장 내부에 존재하는 또 다른 SELECT 문장

- 네스티드 서브쿼리(Nested Subquery)
	- WHERE절에 기술하여 메인쿼리의 조건으로 사용되는 쿼리
	- 주로 EXISTS 또는 IN 연산자를 사용
	- NOT IN 연산자 사용할 때, 서브쿼리 결과에 NULL 존재에 대해 주의 필요  
	> 서브쿼리 내 WHERE절에서 IS NOT NULL로 서브쿼리에 NULL이 없도록 하면 좋음

~~~
SELECT E.EMP_NAME
FROM TBL_EMP E
WHERE E.EMP_ID NOT IN (SELECT M.MANAGER_ID FROM TBL_EMP M WHERE M.MANAGER_ID IS NOT NULL);
~~~
{: .language-sql}

- 인라인 뷰
	- SQL 문장에서 FROM 절에 기술하여 테이블처럼 사용하는 서브쿼리
	- SQL 문장이 실행될 때 생성되어 실행 종료 시 사라지는 임시성 뷰

~~~
SELECT E.EMP_NAME, E.SALARY
FROM TBL_EMP E, 
(SELECT AVG(SALARY) AVGS, MAX(SALARY) MAXS FROM TBL_EMP) B
WHERE E.SALARY BETWEEN B.AVGS AND B.MAXS
ORDER BY A.SALARY DESC;
~~~
{: .language-sql}

- 스칼라 서브쿼리
	- 한 행에서 하나의 열 값을 반환하는 서브쿼리
	- SQL 문장에서 SELECT 컬럼 부분에 기술
	- 스칼라 서브쿼리에 해당 결과가 없다면 NULL을 반환
	- 데이터 양이 적을 때 성능상 유리
	- 하나 이상의 레코드 반환시 오류 발생

~~~
SELECT EMP_NAME, (SELECT DEPT_NAME FROM TBL_DEPT D WHERE E.DEPT_ID = D.DEPT_ID) DEPT
FROM TBL_EMP E;
~~~
{: .language-sql}

- 서브쿼리를 이용한 DML (INSERT, UPDATE 등)
	- 서브쿼리 (SELECT문)에 검색된 행을 입력값으로 사용
	- 한번에 여러 행 입력 가능

~~~
INSERT INTO 테이블명 [(컬럼1, 컬럼2, ...)]
SELECT ...
~~~
{: .language-sql}

~~~
UPDATE 테이블명
SET 
컬럼1 = (SELECT ...),
컬럼2 = (SELECT ...),
...
[WHERE 조건];
~~~
{: .language-sql}

~~~
UPDATE 테이블명
SET 
(컬럼1, 컬럼2, ...) = (SELECT ...)
[WHERE 조건];
~~~
{: .language-sql}


### 계층쿼리

- 자료의 구조가 계층적으로 이루어진 경우, 상위에서 하위 또는 하위에서 상위로 데이터를 조회할 때 사용하는 SQL (트리)
- Oracle에서는 `CONNECT BY`, `START WITH` 키워드로 구현 가능
- Oracle 11g부터 `RECURSIVE WITH` 키워드 지원 (ANSI표준과 호환)

~~~
SELECT 
[LEVEL], 
[CONNECT_BY_ROOT],
[CONNECT_BY_ISCYCLE],
[CONNECT_BY_ISLEAF],
[SYS_CONNECT_BY_PATH],
컬럼1, 
컬럼2, 
...
FROM 테이블명
[WHERE 조건문]
[START WITH 조건문]
[CONNECT BY [NOCYCLE] PRIOR 조건문]
[ORDER SIBLINGS BY 조건문]
;
~~~
{: .language-sql}

> LEVEL : CONNECT BY 조건에 의해 정의된 tree 상 레벨을 파악하는 가상 컬럼  
> START WITH :  계층의 시작위치 지정  
> CONNECT BY [NOCYCLE] PRIOR: 자식과 부모간 relationship 정의  
> - PRIOR 위치에 따라 top-down / bottom-up 형식 지정 가능  
> - CONNECT BY PRIOR 자식키=부모키  
> - NOCYCLE: 루프 존재시에도 로두들을 출력하도록 지시
>
> CONNECT_BY_ISCYCLE : 루프를 발생시키는 ROW파악  
> CONNECT_BY_ISLEAF : LEAF NODE 여부 파악
> CONNECT_BY_ROOT : CONNECT BY 조건에 의해 정의된 TREE 상에서 현재 행의 루트 행의 값 출력
> SYS_CONNECT_BY_PATH : CONNECT BY 조건에 의해 정의된 TREE 상에서 루트 행부터 현재행 까자의 패스 출력
> ORDER SIBLINGS BY : 결과값들의 계층 정보를 유지하며 동일한 부모를 가진 레벨의 행을 정렬

~~~
SELECT EMP_ID, EMP_NAME, MANAGER_ID
FROM TBL_EMP
START WITH MANAGER_ID IS NULL
CONNECT BY PRIOR EMP_ID = MANAGER_ID;
~~~
{: .language-sql}

### TOP N 쿼리

- 쿼리의 결과에서 상위 N 건을 추출하는 쿼리 (RANK쿼리)
- 1건 추출 시 `MIN`, `MAX`함수가 `ORDER BY` 대체 가능

~~~
SELECT EMP_NAME, EMP_BIRTHDAY
FROM TBL_EMP
WHERE EMP_BIRTHDAY = (SELECT MAX(EMP_BIRTHDAY) FROM TBL_EMP);
~~~
{: .language-sql}

### 시퀀스

- 여러사용자가 유니크한 값을 생성할 수 있도록 지원하는 오라클 객체
- `NEXTVAL`과 `CURRVAL`을 사용하여 채번
- Multi User 사용시에도 데이터 중복 없음
- 결번 발생 가능하므로 번호의 연속성이 유지되지 않아도 문제되지 않는 경우 사용
- lock 경합없이 촉각을 다투는 업무에서 사용


~~~
CREATE SEQUENCE 시퀀스이름 
[START WITH n], 
[INCREMENT BY n],
[{MAXVALUE n | NOMAXVALUE}],
[{MINVALUE n | NOMINVALUE}],
[{CYCLE | NOCYCLE}],
[{CACHE n | NOMCACHE}],
[{ORDER | NOORDER}]
;
~~~
{: .language-sql}

> START WITH n : 시퀀스 번호의 시작을 지정  
> INCREMENT BY n : 연속적인 시퀀스 번호의 증가치를 지정  
> MAXVALUE n : 시퀀스의 최대값을 지정  
> MINVALUE n : 시퀀스의 최소값을 지정  
> CYCLE : 시퀀스 값이 최대/최소값 까지 도달하면 START WITH에서 지정한 값으로 시퀀스 다시 시작  
> CACHE n : 메모리 상에서 시퀀스 값을 관리하도록 하는 것으로 기본값은 20  
> ORDER : 시퀀스 번호의 순서 보장 여부를 지정

~~~
INSERT INTO TBL_EMP (EMP_ID, EMP_NAME, ...)
VALUES (EMP_SQ.NEXTVAL, '이름', ...);

INSERT INTO TBL_EMP_DEVICE (EMP_ID, DEVICE_ID, ...)
VALUES (EMP_SQ.CURRVAL, '장비ID', ...);
~~~
{: .language-sql}

### SELECT ... FOR UPDATE ...

- 트랜잭션 수행 시 선택된 ROW 를 다른 트랜잭션이 수정하는 것을 방지하기 위해 사용
- 빈번한 사용 시 처리시간 늦어지고, 시스템 Hang 원인이 됨
- 일반조회시에는 사용 지양, 테이블의 동시성 제어하려 할 때 짧은 시간 동안 사용

|종류|설명|
|-|-|
|FOR UPDATE WITH NO OPTION|Lock을 획득하기 까지 무한 대기<br>트랜잭션 종료(commit, rollback)시 lock해제|
|FOR UPDATE NOWAIT (= WAIT 0)|Lock 획득하지 못하면 바로 에러<br>업무특성상 바로 Lock 획득하고 작업해야 하는 경우 사용<br>실패 시 retry logic 구현 필요|
|FOR UPDATE WAIT X|주어지는 정수 시간만큼 Lock을 획득하기 위해 재시도함<br/>주어진 시간동안 Lock 획득 못하면 에러 발생<br>에러 발생시 retry logic 구현 필요|

~~~
--SESSION 1
SELECT EMP_NAME
FROM TBL_EMP
WHERE EMP_ID = 1000
FOR UPDATE NOWAIT --> 1000인 직원에 대해 명시적 잠금
;

--SESSION 2
SELECT EMP_NAME
FROM TBL_EMP
WHERE DEPT_NO = 10
FOR UPDATE WAIT 5  --> 1000인 직원을 포함해서 부서번호 10인 모든 row에 lock적용
;
--그러나 에러발생.. SS LOCK이 명시적으로 설정된 row를 포함한 레코드에 명시적으로 락을 설정하려면 에러가 발생

--SESSION 2
SELECT EMP_NAME
FROM TBL_EMP
WHERE DEPT_NO = 10
FOR UPDATE SKIP LOCKED  --> 기존에 락이 걸린 row가 제외된 나머지 row가 표시된다.
;
~~~
{: .language-sql}


## SQL 처리 단계

### SQL 처리 단계


1. Create Cursor
	- SQL문장 처리를 위한 메모리 일정 영역 점유
2. Parse SQL
	- SQL 구문 분석과 최적화 통한 실행 계획 생성
3. Bind Variables
	- 구문실행 전, Bind변수를 사용한 경우 변수값 대응
4. Execute SQL
	- DDL, DML 경우 : 구문 자체 실행됨
	- 질의(`QUERY`) : Row Fetch하기 위한 준비수행
5. Fetch Rows
	- Execute 결과값 검색하여 Row 반환
6. Close Cursor
	- SQL 처리된 할당 메모리와 관련된 자원을 반환하며, 공유된 자원(Parsed SQL, Execution Plan 등)은 매모리 내 잔류

## SQL 실행 계획

### 실행계획 개념
- SQL문에서 요구한 사항을 처리하기 위한 절차와 방법
- 최소 일량으로 동일한 일을 처리할 수 있는 최적의 방법을 `OPTIMIZER`가 결정

### 실행계획 필요성
- 시스템의 통계 및 오브젝트 통계정보를 판단기준으로 다양한 액서스 경로를 비교하고, 그 중 가장 효율적인 실행계획이 선택됨
- 실행계획을 검토하여 비효율이 발생한 원인을 확인하고 튜닝의 근거로 사용

### 실행계획 구성요소
- 조인 순서(Join Order)
- 조인 기법(Join Method) : NESTED LOOP, HASH, SORT MERGE
- 액서스 기법(Access Method) : INDEX SCAN, FULL SCAN
- 최적화 정보(Optimization Information)

## 인덱스

### 인덱스 개념

- **어떤 데이터가 어디에 있다**라는 위치정보를 가진 주소록 개념
- 책의 목차나 색인 역할로 테이블과 연결된 물리적인 객체
- 테이블의 Row를 빠르게 access하기 위해 사용
- 인덱스를 생성시킨 Key컬럼과 ROWID(테이블의 Row주소)로 구성되며 정렬된 상태로 저장
- 인덱스는 테이블의 ROW와 하나씩 대응
- `B-Tree인덱스` : 가장 범용적으로 사용하는 인덱스

|데이터블록 엑서스 방식|설명|
|-|-|
|Full Scan(Full Table Scan)|테이블에서 직접 원하는 DATA찾음|
|Index Scan|우선 색인 DATA에서 조건 검색 수행<br>검색된 색인을 사용하여 DATA조회를 수행|

### 인덱스 종류

- Unique Index
	- 중복이 없는 유일한 값을 가지는 컬럼에 대해 생성하는 인덱스
	- 기본키/고유키 무결성 제약조건이 있을 경우 묵시적으로 자동생성

~~~
CREATE UNIQUE INDEX 인덱스명 ON 테이블명(컬럼);
~~~
{: .language-sql}

- Non-Unique Index
	- 중복된 값을 가지는 컬럼에 대해 생성하는 인덱스

~~~
CREATE INDEX 인덱스명 ON 테이블명(컬럼1, 컬럼2, ...);
~~~
{: .language-sql}

- 단일 Index
	- 하나의 컬럼만으로 구성된 인덱트

~~~
CREATE INDEX 인덱스명 ON 테이블명(컬럼);
~~~
{: .language-sql}

- 결합 Index
	- 두 개 이상의 컬럼을 결합하여 생성하는 인덱트
	- 결합 인덱스는 where절 조건 비교에서 두 개 이상의 컬럼이 AND로 연결되고 자주 사용되는 경우에 주로 생성

~~~
CREATE INDEX 인덱스명 ON 테이블명(컬럼1, 컬럼2, ...);
~~~
{: .language-sql}

### 인덱스 사용 지침

- 결합인덱스를 구성하는 컬럼 중, 첫 번째 컬럼이 WHERE 조건에 지정되어야 해당 인덱스가 정상적으로 사용됨
- 테이블의 크기가 적은 것은 인덱스를 사용하지 않는 것이 유리
- 넓은 범위를 인덱스로 처리 시 성능을 감소시킬 수 있음
- 새로 추가된 인덱스는 기존 액서스 경로에 영향을 미칠 수 있으므로, 신규 인덱스가 추가될 경우, 애플리케이션 상에서 영향도 확인 필요

### 인덱스가 사용되지 않는 경우

- Index 구성컬럼의 외부적 변형
- Index 구성컬럼의 내부적 변형
- 부정형 비교
- NULL값 비교
- 부적절한 LIKE 비교
- HAVING 절 조건 추가

## 조인처리방식

### Nested Loop Join

- 선행 테이블의 데이터와 매치되는 값을 후행 테이블에서 찾아오는 방식
- 선행 테이블에서 추출되는 데이터 건수만큼 후행 테이블을 반복 액서스

### Sort Merge Join

- 두 테이블을 각각 읽어 조인 컬럼을 정렬한 후, 결과를 합쳐 조인을 하는 방식
- 항상 정렬작업이 발생하므로 정렬크기에 따라 시스템 자원 소모가 클 수 있음

### Hash Join

- 선행 조인 컬럼의 Hash테이블을 메모리에 만들고, 후행 테이블의 조인 컬럼과 일치하는 값을 선별하는 방식
- Sort Merge의 시스템 자원소모의 대안으로 등장


## 정규화

### 정규화 개요

- 관계형 데이터베이스의 설계에서 **중복을 최소화**하여 데이터를 구조화하는 프로세스
- 정규화 목표는 이상있는 관계를 재구성하고 작고 잘 조직된 관계를 생성
- 데이터 무결성 유지 및 안정성 최대화
- 제 1정규화, 제 2정규화, 제 3정규화, BCNF 등.... (4, 5, 6 정규형도 있으나...)


|++제품번호++|제품명|재고수량|주문번호|고객번호|주소|주문수량|
|---|---|---|---|---|---|---|
|1001|모니터|2000|A345|100|서울|150|
|1001|모니터|2000|D347|200|부산|300|
|1007|마우스|9000|A210|300|광주|600|
|1007|마우스|9000|A345|100|서울|400|
|1007|마우스|9000|B230|200|부산|700|
|1201|키보드|2100|D347|200|부산|300|

*정규화 전 테이블 : TBL_주문목록*

### 제1정규화

- 테이블에 있는 모든 속성의 도메인이 원자값(Atomic Value) 만으로 되어있는 정규형
- 반복되는 그룹속성이 존재할 경우, 그 그룹을 분리하여 새로운 Entity타입을 추가한 후 기존의 실체와 1:N의 관계를 형성해준다. (제품명, 재고수량, 주문번호)
- 하나의 제품에 대해 여러개의 주문관련정보(주문번호, 고객번호, 주소, 주문수량)발생


|++제품번호++|제품명|재고수량|
|---|---|---|
|1001|모니터|2000|
|1007|마우스|9000|
|1201|키보드|2100|

*제1정규화 후 테이블 : TBL_제품목록*

|++주문번호++|++제품번호++|고객번호|주소|주문수량|
|---|---|---|---|---|
|A345|1001|100|서울|150|
|A345|1007|100|서울|400|
|D347|1001|200|부산|300|
|D347|1201|200|부산|300|
|B230|1007|200|부산|700|
|A210|1007|300|광주|600|

*제1정규화 후 테이블 : TBL_주문목록*

### 제2정규화

- 기본키 2개 이상으로 구성되는 테이블에서 일부 속성에 대해서만 부분적으로 함수 종속적인 것을 분리
- 즉, **부분함수 종속성**을 제거해준다 (만약 기본키가 하나인 경우 제2정규화는 하지 않는다)
- `TBL_주문목록`에서 함수종속성 발생
	- 주문번호, 제품번호 -> 고객번호, 주소, 주문수량
	- 주문번호 -> 고객번호, 주소 ==**부분 종속성 발생**==


|++제품번호++|제품명|재고수량|
|-|-|-|
|1001|모니터|2000|
|1007|마우스|9000|
|1201|키보드|2100|

*제2정규화 후 테이블 : TBL_제품목록 (수정 없음)*

|++주문번호++|고객번호|주소|
|-|-|-|
|A345|100|서울|
|D347|200|부산|
|B230|200|부산|
|A210|300|광주|

*제2정규화 후 테이블 : TBL_주문*

|++주문번호++|++제품번호++|주문수량|
|-|-|-|
|A345|1001|150|
|A345|1007|400|
|D347|1001|300|
|D347|1201|300|
|B230|1007|700|
|A210|1007|600|

*제2정규화 후 테이블 : TBL_주문목록*

### 제3정규화

- 기본키에 의존하지 않고 일반 컬럼에 의존하는 컬럼을 제거
- A -> B, B -> C 그러므로 A -> C 즉, **이행적 종속관계**를 분리
- `TBL_주문`에서 이행적 종속관계 발생
	- 주문번호 -> 고객번호, 주소
	- 고객번호 -> 주소 ==**일반키에서 함수적 종속 발생**==

|++제품번호++|제품명|재고수량|
|-|-|-|
|1001|모니터|2000|
|1007|마우스|9000|
|1201|키보드|2100|

*제3정규화 후 테이블 : TBL_제품목록 (수정 없음)*

|++고객번호++|주소|
|-|-|
|100|서울|
|200|부산|
|300|광주|

*제3정규화 후 테이블 : TBL_고객*

|++주문번호++|++고객번호++|
|-|-|
|A345|100|
|D347|200|
|B230|200|
|A210|300|

*제3정규화 후 테이블 : TBL_주문*

|++주문번호++|++제품번호++|주문문수량|
|-|-|-|
|A345|1001|150|
|A345|1007|400|
|D347|1001|300|
|D347|1201|300|
|B230|1007|700|
|A210|1007|600|

*제3정규화 후 테이블 : TBL_주문목록 (수정 없음)*

---

## References

[정규화 참고1](http://hahahia.tistory.com/147)<br>
[정규화 참고2](http://magazine.infoever.co.kr/letter_09/0812/data/200908_modeling.pdf)