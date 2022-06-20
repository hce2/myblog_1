---
title: 8장 PL/SQL의 구조와 구성요소 살펴보기
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: ORACLE
---
## 01. PL/SQL 기본 구조

- 선언부

```sql
-- 선언부
SET SERVEROUTPUT ON
SET TIMING ON -- 경과시간 확인
DECLARE
    vi_num NUMBER; -- 변수 선언
BEGIN 
    -- 실행 (코드 실행)
    vi_num := 100;
    DBMS_OUTPUT.PUT_LINE(vi_num);
END; -- 하나의 블록이 종료
/ -- PL/SQL 자체가 종료
-- SQL을 스크립트로 파일 형태로 실행 (Bash 터미널) 할 때 '/' 반드시 필요
```

❶ **이름부:** 블록의 명칭이 오는데, 생략할 때는 익명 블록이 된다.

❷ **선언부:** DECLARE로 시작되며, 실행부와 예외 처리부에서 사용할 각종 변수, 상수, 커서 등을 선언한다. 중요한 점은 변수 선언이나 실행부와 예외 처리부에서 사용하는 각종 문장의 끝에 반드시 **세미콜론**(;)을 찍어야 한다는 것이다. 세미콜론이 없으면 오류가 발생한다. 사용할 변수나 상수가 없다면 선언부를 생략할 수 있다.

❸ **실행부:** 실제 로직을 처리하는 부분이다. 이 부분에는 각종 문장(일반 SQL문, 조건문, 반복문 등)이 올 수 있고, 이러한 문장을 사용해 비즈니스 로직을 구현하는 것이다. 하지만 SQL 문장 중 DDL문은 사용할 수 없고 DML 문만 사용할 수 있으며 모든 문장의 끝에는 세미콜론을 붙여야 한다.

❹ **예외 처리부: EXCEPTION** 절로 시작되는 부분으로 실행부에서 로직을 처리하다가 오류가 발생하면 처리할 내용을 기술하는 부분으로 예외 처리부는 생략이 가능하다. ‘예외처리’ 장에서 자세히 설명하겠지만, PL/SQL에서 말하는 오류는 크게 두 가지로 나눌 수 있다. PL/SQL 코드 컴파일 과정에서 발생하는 오류와 런타임, 즉 실행 과정에서 발생하는 오류가 있는데, 예외(exception)이란 바로 런타임 오류를 말하는 것으로 예외 처리부에서는 런타임 오류가 발생했을 때 처리할 부분을 기술한다. 예를 들어, 매개변수로 다른 임의의 수를 나누는 로직이 있는데 만약 이 매개변수에 0이 들어오면 오류가 발생할 것이다. 이때 “0으로 나눌 수 없습니다”라는 메시지를 보여주는 처리를 예외 처리부에 기술해 주는 것이다.

## 02. PL/SQL 구성 요소

- 변수

```sql
DECLARE
    a INTEGER := 2**2*3**2; -- 4 * 9
BEGIN
    DBMS_OUTPUT.PUT_LINE('a = ' || TO_CHAR(a));
END;
```

- DML문 : PL/SQL 테이블과 연동해서 특정 로직을 처리하는 것

```sql
DECLARE 
    vs_emp_name VARCHAR2(80); -- 사원명 변수
    vs_dep_name VARCHAR2(80); -- 부서명 변수 
BEGIN 
    SELECT a.emp_name, b.department_name
        INTO vs_emp_name, vs_dep_name
    FROM employees a 
        , departments b 
    WHERE a.department_id = b.department_id
        AND a.employee_id = 100; 
    DBMS_OUTPUT.PUT_LINE(vs_emp_name || ' - ' || vs_dep_name);
END;
```

테이블에 있는 데이터를 선택해 변수에 할당할 때는 **SELECT 문에서 INTO절을 사용**한다는 점

- 데이터타입
    - **%TYPE**
     키워드를 쓰면 해당 변수에 컬럼 타입을 자동으로 가져온다.

```sql
DECLARE 
    vs_emp_name employees.emp_name%TYPE; -- 사원명 변수
    vs_dep_name departments.department_name%TYPE; -- 부서명 변수 
BEGIN 
    SELECT a.emp_name, b.department_name
        INTO vs_emp_name, vs_dep_name
    FROM employees a 
        , departments b 
    WHERE a.department_id = b.department_id
        AND a.employee_id = 100; 
    DBMS_OUTPUT.PUT_LINE(vs_emp_name || ' - ' || vs_dep_name);
END;
```

- SQL과 PL/SQL 데이터 타입별 길이
    - SQL과 PL/SQL에서 사용할 수 있는 데이터 크기는 다름
    - PL/SQL > SQL보다 더 큰 크기로 사용 가능

```sql
DROP TABLE ch08_varchar2;
CREATE TABLE ch08_varchar2(
    VAR1 VARCHAR2(4000)
);

INSERT INTO ch08_varchar2 (VAR1)
VALUES ('tQbADHDjqtRCvosYCLwzbyKKrQCdJubDPTHnzqvjRwGxhQJtrVbXsLNlgeeMCemGMYpvfoHUHDxIPTDjleABGoowxlzCVipeVwsMFRNzZYgHfQUSIeOITaCKJpxAWwydApVUlQiKDgJlFIOGPOKoJsoemqNbOLdZOBcQhDcMLXuYjRQZDIpgpmImgiwzcLkSilCmLrSbmFNsKEEpzCHDylMvkYPKPNeuJxLvJiApNCYzrMcflECbxwNTKSxaEwVvCYnTnFfMFgDqxobWcSmMJrNTQIVOeWlPaMTfRHsrlFSukppmljmOojPSgJiSbQcgtWWOwUNNYFGtgCGBsIcTGAiHWBxtYVXecoJgJCAJptIVmVTZSKliRLoPYTIUpksBuQaqFHLhCkosWChoMjbqgLtBIRBynsKjKiLrdeHVvZanNVElDjLWwlCDhbpsAVQMTzjzhoKIJBdthynMBMVjeNmsKAjdAYhPZKmuKOuMloQdkqPjoKbfjDEeATciMrXiMQorMhYmBlMODBbyLLIkbmtZdPcWGSuxFEUwXnWpvnunEgcLelSneRIpgRNTzTkHqgLbpxoHzCYgSWlIAvKljCnmWiPWGGwlUFOudRSdoqUxntyhNYEiVXtMObywEltTImawnElpmeiWwlTjGTFceqyjhNqiDLxwduubykWzDmFSJNvVvDZibrCpAReqQjlQZcxuVqjKGKvoDuEcQPQeDzmdMYSOTIQdPDNfDffCOUWflHSQhvVTiYumBQIoyznWNITGZkefknJpGEutUnhBgLPQTWTBeTYccqlLrxvRjfJpdpfVDqqfKCngemIEDDHNdvBxCqKDTrrJAumXMKgpWLIHctQuACeNaKnffpYXiioLxZDrxpuZPPUGpRsCtoQuBfogkKuusVATkMyajKTPSyTQbfhZepRjNdrhkymqKvsAcThYbMSMnkKcLWFPAMeGysBVKkQtFMPvRBoDszlSZcMYzwxkKQwJnuVnDxShYiHFlzgDWqhZoqeypyFVBNDtHkiVzHkQisYLbsbVneJyHbHdtaIFLVbfTqbkGQTEjFlPiGUddPUIoLWALrbKcLwBizwhJvaXkvOphcGWpdNAhxgehCvjcQFSFhxrBuANKjyWncWAUpKKJcfQCsQlLfpqdMhjWGkAMMWUaDfCrGtmtkiIZOdNapEnvfFKiHAhBhejgKSuyKXFQXyCaLwwvonHsceJKgjtnYVZvBCYYBSqNCqVqCGewootJJsqrCnmiteMZBbyMPnIrdcielnGUYmwiOPmEqKGvxDmDRTDRumnSRcnvgxLbaiQIuzdslEIMquvvwmvgaumqPkduNyfRtXErCPvDYLelhjNNOjbGryRpTtDHxIJebMEtKryUyZRIdADeTEBExwHMRHzAYFizYiesaMhNIsOUzUTmyEMuFQrsUEtjwhUWIvADNlrcxPZwRazPMMvdVZssmXbXuCkRoPYNGLPwUmrWrrIgQoMSGMPvTcbHnbtleyKYmOMgymANQBZDMoqAOzMHrAVunIiykCudFVNObNgXOoyfQRICbFsWygSZXufipvrWWmRnBWYdoKmIRewOObUjiNDdQsxQIXtlbPSSngfQPfeQKOolVASXIuAmeODKtSOPaEaFKcedGzzsbrPlsPnRRuYFeVdhyufpjFVVrTPczSQkmPYXercLMmVEaDmJXKTqEVNSKeOshDCDJwdINFsLhAuKIIfOdjSEndDwumQLvePVjzNoIfUELOANeshoNgwVhFADjtUIjIhQAIyRnzSoxSRSWklITMgdjQZTthwsnBVLWyfSsAdLzOnEqmMCGBlTYGjtqvKbBoATRwkPkOTSbUhZClVzjiLLIFEMuptuodeRKXUaBfUhVTtasFsZdVnKtEfLldJYsxjlrBADRqhEBEmBKxlXKgEhiKcwAdztcETMUteJwadfaZLEBRjwJOGaIMhsfAxtuBQWyQLGXPDlFQmkcMsKsGUlQBEAubDqbuBYqXLZgmhPftLkYaCYGReLCVXssOxzJFJwnxKJzaaYzfVpbHYBtiBeQZRilJZqrrMTrVtYAcwGxAAddwtlxzdZebfZHjzqRmrrBPNbkVHqjCHtVKUjIDPVSrtyEsPRPoyyPOFOSBcgClTzlAIPmPMkdlpFHctzKGpyQMInMwPKojVErCOrHbCsZoEXqyOcHReSybmxwYabyioVnDxPEvskutVHLWQTNudmKICoaoSGKqONrBmvtGNBKAaJxCRKTDOIqrJOsQVOmGxmuIDEddVYvDwILTyushOAiXbkRIKgNLnFJdOagmiOHKRBKIIkxkOUeZWMRNlqpJdFgKjrGhIzrgBtgjVOtZAskKRbqzRVwLUoUAtRpRkoRQNLIrbLmmjZTugXJBNCscnMguKVAFDKpODtCsmdlBvQGALeBGUitYBxLYhJxeVcAnTWmTAvCITzdzqiBfEudEIBmkDAXIFmoOmsTMZDOnhXYrgMDlDbjednYWWJbGhrXFrxMQmQSmRBwoOqWGbGmjZNlJCvSHvmtZUkIScWXVdfSsdvdyQNpGFIOuteXhCMLmmEHrMucEmFbCIOHTJINAuIUOPfAfijIPkZjppGCCSRJNXWNCmliwUgABkHWuelUWeLsyVKVcZWOSeiQBQibCQJQUgGkTrXZxdBLsgjeMIwOyORDBpywuvlrLScRNhvaCYaKKRvOZeqBebUWWFhNnIRJvedFNfFPgWZJgNRaUpyYWFNiXJfAqNjyCEQYwAdFBQKKolwrufmJOfrToJFEsoNjaphcNvfWGIjKrKZSoSJEsbRqNVcoprpcGrnBgcNAnWUFpRldcPJkPfaoLKRCmVyMAWMXmnScodKisCTqllZEWQQSCFETxLNntgdcFEFRsTSIhuewwrHIlOeCcRqkzgQhKnKyHZHdFsMEKvPywLbjaspVxUMEkVzCGcGoTmaBjUMwJuAYdSTaYGDHHWDrvGgMVTtehpzfgofkmqtamffJbCKOzJgPsHNEnFarjADJGyKLwwitCiBXIraUdZtZwNjUtGbWqxksepVYztIBrimByoYQfUQgOndzFmhnuSmhYWvHliWUHgbvBIkYasDElNsjcCLtMvjQEhJjWvlnAscPwOYfelrfgfRAZGBxdFlMNkfYEWLbkfUhbRPHoDZsaAQdoKhAAWzOcHoAkkHPQMNIxgHNJaqEFBqCuMYEtLpMnIiMCWWEPnBYgYrxlXFGYpQWUNFevwcEUvUzDeSZNrdmahAfjeLSAGjHVnqyTzJkiVXjDJXzOiszXQCErQwwDMMqjLxWebJwNAVdrXeyMDRYXmLMDnuWLVaShVGhlgvbjOdOnhCDTNVazYDnzstqxjOuWbLcDaavRumKUOQXBQwKtdFgOzXiQKWFporrIcylIHlTmTKAIpBqNUbkajLTlwAHieCcqPIJYhegwQhWpYZdfxpQXDKtYzsrmnvdiTKgXfXKlIHPHlxQtqXGhMVPOBAKVZJfkrDNEwnQFwgfoHJSqQxTzRswVLrtFgpVzKcLilgznElWUfhERyeUrCcFCuGJddlFHJrXsqRdUjqUwaBmJVNwjRbCFiVMOSFuNctNVzhmhUpoddsMPUFMvNIMsMjHIWYiLjhSajZqpDkMvUOUCbYKfNHGpdUeWGUtDXHDNSCEXqYrhWhvnISnjfoBMCwwptksarPImRZaRxBMjoBdlmRGlIuQZDzCLnxxioATnGVFFTATUpeypOCaCeJAvPLxEXYzlCgXvXirGSZFyZPPSCdOSHxeELRsetFrWgqPNNpwgbgBEYPOSpLWeVdqOxPaQnidyPVMmELzeJPWgNsWBdPJPjhkdGpeAYZfrBNqdbOwzbtLiWMPafjgWQNcWKqmcleWLcMJoGSAEIUyFuzElZKXonHOMDdGMtSKEFUWdfPfnDecKNhIjAKRYmkXgpPAzlKIOpViZPkZdozzAoWwDnXkfDikvkXcQaoBtzKkcRhNpJRYaGTkdnlfotsJZsLqpYaWoK');

commit;
    
DECLARE
   vs_sql_varchar2   VARCHAR2(4000);
   vs_plsql_varchar2 VARCHAR2(32767);
BEGIN

    -- ch08_varchar2 테이블의 값을 변수에 담는다. 
    SELECT VAR1 
        INTO vs_sql_varchar2
    FROM ch08_varchar2;
     
    -- PL/SQL 변수에 4000 BYTE 이상 크기의 값을 넣는다. 
    vs_plsql_varchar2 := vs_sql_varchar2 || ' - ' || vs_sql_varchar2 || ' - ' || vs_sql_varchar2;
  
    -- 각 변수 크기를 출력한다. 
    DBMS_OUTPUT.PUT_LINE('SQL VARCHAR2 길이 : ' || LENGTHB(vs_sql_varchar2)); 
    DBMS_OUTPUT.PUT_LINE('PL/SQL VARCHAR2 길이 : ' || LENGTHB(vs_plsql_varchar2)); 
END;
```

- 두 수의 합

```sql
accept p_num1 prompt '첫번째 숫자를 입력하세요'
accept p_num2 prompt '첫번째 숫자를 입력하세요'

DECLARE
    v_sum number(10);
BEGIN
    v_sum := &p_num1 + &p_num2;
    DBMS_OUTPUT.PUT_LINE('Total :' || v_sum);
END;
```

---

- 데이터 입력

```sql
DROP table emp;
DROP table dept;

CREATE TABLE DEPT
       (DEPTNO number(10),
        DNAME VARCHAR2(14),
        LOC VARCHAR2(13) );

INSERT INTO DEPT VALUES (10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO DEPT VALUES (20, 'RESEARCH',   'DALLAS');
INSERT INTO DEPT VALUES (30, 'SALES',      'CHICAGO');
INSERT INTO DEPT VALUES (40, 'OPERATIONS', 'BOSTON');

CREATE TABLE EMP (
 EMPNO               NUMBER(4) NOT NULL,
 ENAME               VARCHAR2(10),
 JOB                 VARCHAR2(9),
 MGR                 NUMBER(4) ,
 HIREDATE            DATE,
 SAL                 NUMBER(7,2),
 COMM                NUMBER(7,2),
 DEPTNO              NUMBER(2) );

INSERT INTO EMP VALUES (7839,'KING','PRESIDENT',NULL,'81-11-17',5000,NULL,10);
INSERT INTO EMP VALUES (7698,'BLAKE','MANAGER',7839,'81-05-01',2850,NULL,30);
INSERT INTO EMP VALUES (7782,'CLARK','MANAGER',7839,'81-05-09',2450,NULL,10);
INSERT INTO EMP VALUES (7566,'JONES','MANAGER',7839,'81-04-01',2975,NULL,20);
INSERT INTO EMP VALUES (7654,'MARTIN','SALESMAN',7698,'81-09-10',1250,1400,30);
INSERT INTO EMP VALUES (7499,'ALLEN','SALESMAN',7698,'81-02-11',1600,300,30);
INSERT INTO EMP VALUES (7844,'TURNER','SALESMAN',7698,'81-08-21',1500,0,30);
INSERT INTO EMP VALUES (7900,'JAMES','CLERK',7698,'81-12-11',950,NULL,30);
INSERT INTO EMP VALUES (7521,'WARD','SALESMAN',7698,'81-02-23',1250,500,30);
INSERT INTO EMP VALUES (7902,'FORD','ANALYST',7566,'81-12-11',3000,NULL,20);
INSERT INTO EMP VALUES (7369,'SMITH','CLERK',7902,'80-12-11',800,NULL,20);
INSERT INTO EMP VALUES (7788,'SCOTT','ANALYST',7566,'82-12-22',3000,NULL,20);
INSERT INTO EMP VALUES (7876,'ADAMS','CLERK',7788,'83-01-15',1100,NULL,20);
INSERT INTO EMP VALUES (7934,'MILLER','CLERK',7782,'82-01-11',1300,NULL,10);

commit;

drop  table  salgrade;

create table salgrade
( grade   number(10),
  losal   number(10),
  hisal   number(10) );

insert into salgrade  values(1,700,1200);
insert into salgrade  values(2,1201,1400);
insert into salgrade  values(3,1401,2000);
insert into salgrade  values(4,2001,3000);
insert into salgrade  values(5,3001,9999);

commit;
```

- 사원 번호를 찾아라

```sql
SELECT * FROM emp;
```

- 사원 번호를 입력하면 해당 사원의 급여가 나오도록 출력하세요
    - 7782

```sql
ACCEPT p_empno prompt '사원 번호를 입력하세요'
DECLARE
    v_sal number(10);
BEGIN
    SELECT SAL INTO v_sal
    FROM emp
    WHERE empno = &p_empno;
    DBMS_OUTPUT.PUT_LINE('월급은 ' || v_sal);
END;
```