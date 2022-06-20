---
title: 4장. SQL 함수 살펴 보기
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: ORACLE
---
## 01. 숫자 함수

- ABS(n) : 절대값 반환하는 함수

```sql
SELECT
    ABS(10)
    , ABS(-10)
    , ABS(10.123)
    , ABS(-10.123)
FROM DUAL; /* 시스템에 기본으로 있는 테이블 */
```

![Untitled](images/oracle4/Untitled.png)

- CELL &  FLOOR
    - CELL : 올림, FLOOR : 내림

```sql
SELECT 
    CEIL(10.123)
    , CEIL(-10.123)
    , FLOOR(10.123)
    , FLOOR(-10.123)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%201.png)

- ROUND : 반올림

```sql
SELECT
    ROUND(10.154)
    , ROUND(10.151)
    , ROUND(11.001)
    , ROUND(-10.154)
    , ROUND(-10.151)
    , ROUND(-11.001)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%202.png)

```sql
SELECT
    ROUND(10.154, 1)
    , ROUND(10.151, 2)
    , ROUND(-10.154, 1)
    , ROUND(-10.151, 2)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%203.png)

```sql
SELECT 
    ROUND(0, 3)
    , ROUND(115.155, -1) /* 소숫점 앞의 5에서 반올림 */
    , ROUND(115.155, -2) /* 백의 자리 1에서 반올림 */
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%204.png)

- TRUNC : 반올림 안함. 소숫점 절삭

```sql
SELECT
    TRUNC(115.155)
    , TRUNC(115.155, 1)
    , TRUNC(115.155, 2)
    , TRUNC(115.155, -2)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%205.png)

- POWER(n2, n1) & SQRT(n)
    - 제곱 & 제곱근

```sql
SELECT
    POWER(3, 2)
    , POWER(3, 3)
    , SQRT(9)
    , SQRT(8)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%206.png)

- MOD(n2, n1) & REMAINDER(n2, n1)
    - MOD : n2를 n1으로 나눈 나머지 값 반환
    - REMAINDER : n2를 n1으로 나눈 나머지 값 반환하는데, 나머지를 구하는 내부적 연산 방법이 MOD와는 약간 다르다
    - **MOD → n2 - n1 * FLOOR (n2/n1)**
    - **REMAINDER → n2 - n1 * ROUND (n2/n1)**

```sql
SELECT
    MOD(19, 4)
    , REMAINDER(19, 4)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%207.png)

- EXP, LN, LOG
    - EXP : 지수 함수, e의 n제곱 값을 반환
    - LN : 자연 로그 함수, 밑수가 e인 로그 함수
    - LOG : n2를 밑수로 하는 n1의 로그 값 반환

```sql
SELECT 
    EXP(2)
    , LN(2.713)
    , LOG(10, 100)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%208.png)

## 02. 문자 함수

- CONCAT : '||' 연산자처럼 두 문자를 붙여 반환

```sql
SELECT CONCAT('I Have', ' A Dream'), 
    'I Have' || ' A Dream'
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%209.png)

- SUBSTR : 문자 개수 단위로 문자열 자름

```sql
SELECT 
    SUBSTR('ABCDEFG', 1, 4) /*인덱스가 1부터 시작*/
    , SUBSTR('ABCDEFG', -1, 4)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2010.png)

- SUBSTRB : 문자열의 바이트 수만큼 자름

```sql
SELECT
    SUBSTRB('ABCDEFG', 1, 4)
    , SUBSTRB('가나다라마바사', 1, 4)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2011.png)

- LTRIM(char, set) & RTRIM(char, set)
    - 특정한 문자의 왼쪽, 오른쪽을 제거한 후 반환

```sql
SELECT
    LTRIM('ABCDEFGABC', 'ABC')
    , LTRIM('가나다라', '가')
    , RTRIM('ABCDEFG', 'ABC')
    , RTRIM('가나다라', '라')
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2012.png)

- LPAD & RPAD
    - LPAD : 문자열 맨 왼쪽에 무언가를 입력해준다.
    - RPAD : 문자열 맨 오른쪽에 무언가를 입력해준다.

```sql
CREATE TABLE ex4_1(
    phone_num VARCHAR2(30)
);

INSERT INTO ex4_1 VALUES ('111-1111');
INSERT INTO ex4_1 VALUES ('111-2222');
INSERT INTO ex4_1 VALUES ('111-3333');

SELECT * FROM ex4_1;
```

![Untitled](images/oracle4/Untitled%2013.png)

```sql
SELECT LPAD(phone_num, 12, '(02)') FROM ex4_1;
```

![Untitled](images/oracle4/Untitled%2014.png)

```sql
SELECT RPAD(phone_num, 12, '(02)') FROM ex4_1;
```

![Untitled](images/oracle4/Untitled%2015.png)

## 03. 날짜 함수

- SYSDATE & SYSTIMESTAMP
    - 현재일자와 시간을 DATE, TIMESTAMP 형으로 반환

```sql
SELECT 
    SYSDATE
    , SYSTIMESTAMP
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2016.png)

- ADD_MONTHS : 매개변수로 들어온 날짜에 interger 만큼의 월을 더한 날짜를 반환

```sql
SELECT
    ADD_MONTHS(SYSDATE, 1)
    , ADD_MONTHS(SYSDATE, -1)
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2017.png)

- MONTHS_BETWEEN(date1, date2): 두 날짜 사이의 개월 수 반환, date2가 date1보다 빠른 날짜가 온다.

```sql
SELECT  
    MONTHS_BETWEEN(SYSDATE, ADD_MONTHS(SYSDATE, 1)) mon1
    ,MONTHS_BETWEEN(ADD_MONTHS(SYSDATE, 1), SYSDATE) mon2
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2018.png)

- LAST_DAY : 해당 날짜를 기준으로 월의 마지막 일자를 반환

```sql
SELECT 
    LAST_DAY(SYSDATE)
    , LAST_DAY(ADD_MONTHS(SYSDATE, 1))
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2019.png)

- ROUND(date, format) & TRUNC(date, format)
    - ROUND : fotmat에 따라 반올림한 날짜
    - TRUNC : 잘라낸 날짜를 반환

```sql
SELECT
    SYSDATE
    , ROUND(SYSDATE, 'month')
    , TRUNC(SYSDATE, 'month')
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2020.png)

- NEXT_DAY (date, char) : date를 char에 명시한 날짜로 다음 주 주중 일자를 반환

```sql
SELECT NEXT_DAY(SYSDATE, '금요일')
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2021.png)

## 04. 변환 함수

- TO_CHAR(숫자 혹은 날짜, format) : 숫자나 날짜를 문자로 변환, format에는 특정 형식을 쓸 수 있다.

```sql
SELECT TO_CHAR(123456789, '999,999,999')
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2022.png)

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD')
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2023.png)

매개변수로 오는 숫자나 날짜에 따라 자주 사용하는 format을 정리하면 다음과 같다.

[날짜 변환 형식](images/oracle4/%E1%84%82%E1%85%A1%E1%86%AF%E1%84%8D%E1%85%A1%20%E1%84%87%E1%85%A7%E1%86%AB%E1%84%92%E1%85%AA%E1%86%AB%20%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%B5%E1%86%A8%20c5c0245b7b4b458993bec91cdcd46284.csv)

[숫자 변환 형식](images/oracle4/%E1%84%89%E1%85%AE%E1%86%BA%E1%84%8C%E1%85%A1%20%E1%84%87%E1%85%A7%E1%86%AB%E1%84%92%E1%85%AA%E1%86%AB%20%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%B5%E1%86%A8%208064010dd3e24d36bb46fef73cb8bc10.csv)

- TO_NUMBER(expr, format) : 문자나 다른 유형의 숫자를 NUMBER 형으로 변환하는 함수

```sql
SELECT TO_NUMBER('123456')
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2024.png)

- TO_DATE, TO_TIMESTAMP : 문자를 날짜형으로 변환
    - TO_DATE는 DATE형으로, TO_TIMESTAMP는 TIMESTAMP형으로 변환

```sql
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD HH24:MI:SS';
SELECT TO_DATE('20140101', 'YYYY-MM-DD')
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2025.png)

```sql
SELECT TO_DATE('20220427 14:06:10', 'YYYY-MM-DD HH24:MI:SS') 
FROM DUAL;
```

![Untitled](images/oracle4/Untitled%2026.png)

## 05. NULL 관련 함수

- NVL(expr1, expr2) : expr1이 NULL일 때 expr2를 반환

```sql
SELECT 
    NVL(manager_id, employee_id)
FROM employees
WHERE manager_id IS NULL;
```

![Untitled](images/oracle4/Untitled%2027.png)

manager_id가 NULL인 사원을 조회했는데, manager_id가 NULL일 때 manager_id 대신 사번(employee_id)을 조회하는 쿼리다.

- NVL2(expr1, expr2, expr3) : expr1이 NULL이 아니면 expr2를 expr1이 NULL이면 expr3을 실행

```sql
DESC employees;
SELECT 
    employee_id 
    , commission_pct
    , salary
    , NVL2(commission_pct
           , salary + (salary * commission_pct)
           , salary) as final_salary
FROM employees;
```

![Untitled](images/oracle4/Untitled%2028.png)

커미션(commission_pct)이 NULL인 사원은 그냥 급여를, NULL이 아니면 ‘급여 + (급여 * 커미션)’을 조회하고 있다.

- COALESCE(expr1, expr2, ...) : 매개변수로 들어오는 표현식에서 NULL이 아닌 첫번째 표현식 반환

```sql
SELECT 
    employee_id
    , salary
    , commission_pct
    , COALESCE(salary * commission_pct, salary) AS salary2
FROM employees;
```

![Untitled](images/oracle4/Untitled%2029.png)

‘급여*커미션’ 값이 NULL이면 salary를, NULL이 아니면 ‘급여*커미션’ 값을 반환하고 있다.

- LNNVL(조건식)
    - 매개변수로 들어오는 조건식의 결과 FALSE나 UNKNOWN --> TRUE 반환
    - TRUE 이면 FALSE로 반환
- 커미션이 0.2 이하인 사원을 조회

```sql
SELECT 
    employee_id
    , commission_pct
FROM employees
WHERE commission_pct < 0.2;
```

![Untitled](images/oracle4/Untitled%2030.png)

커미션이 NULL인 사원도 0.2 이하라고 봐야 하는데 위 쿼리 결과에는 조회되지 않는다. NVL 함수를 써서 커미션이 NULL인 사원은 0을 반환하도록 하면 구할 수 있다.

- 커미션이 NULL인 사원은 0을 반환

```sql
SELECT 
    -- count(*) -- 행 갯수 반환
    employee_id
    , commission_pct
FROM employees
WHERE NVL(commission_pct, 0) < 0.2;
```

![Untitled](images/oracle4/Untitled%2031.png)

- LNNVL 함수를 써서도 동일한 결과가 출력되도록 구현할 수 있다.

```sql
SELECT 
    count(*) -- 행 갯수 반환
    -- employee_id
    -- , commission_pct
FROM employees
WHERE LNNVL(commission_pct >= 0.2);
```

![Untitled](images/oracle4/Untitled%2032.png)

- NULLIF (expr1, expr2) : expr1과 expr2을 비교해 같으면 NULL을, 같지 않으면 expr1을 반환

```sql
SELECT 
    employee_id
    , TO_CHAR(start_date, 'YYYY') start_year
    , TO_CHAR(end_date, 'YYYY') end_year
    , NULLIF(TO_CHAR(end_date,'YYYY'), TO_CHAR(start_date, 'YYYY')) nullif_year
FROM job_history;
```

![Untitled](images/oracle4/Untitled%2033.png)

job_history 테이블에서 start_date와 end_date의 연도만 추출해 두 연도가 같으면 NULL을, 같지 않으면 종료년도를 출력하는 쿼리다.

## 06. 기타 함수

-