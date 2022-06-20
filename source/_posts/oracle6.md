---
title: 6장. 테이블 사이를 연결해 주는 조인과 서브 쿼리 알아보기
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: ORACLE
---
- 동등 조인 : WHERE 절에서 '=' 연산자를 사용해 조인

```sql
SELECT
a.employee_id
, a.emp_name
, a.department_id
, b.department_name
FROM
employees a
, departments b
WHERE a.department_id = b.department_id;
```

```sql
- 세미 조인

-- 서브쿼리를 사용함
-- 서브 쿼리에 존재하는 데이터만 메인 쿼리에서 추출
-- IN & EXISTS
```

```sql
- EXISTS
SELECT
department_id
, department_name
FROM departments a
WHERE EXISTS (SELECT *
FROM employees b
WHERE a.department_id = b.department_id
AND b.salary > 3000)
ORDER BY a.department_name;
```

```sql
- IN 사용
SELECT
department_id
, department_name
FROM departments a
WHERE a.department_id IN (SELECT
b.department_id
FROM employees b
WHERE b.salary > 3000)
ORDER BY a.department_name;
```

```sql
- 안티 조인
-- 세미 조인 개념의 반대
-- 서브쿼리의 B 테이블에 없는 메인 쿼리
```

```sql
SELECT
a.employee_id
, a.emp_name
, a.department_id
, b.department_name
FROM
employees a
, departments b
WHERE a.department_id = b.department_id
AND a.department_id NOT IN (SELECT department_id
FROM departments
WHERE manager_id IS NULL); -- 결측치가 아닌 친구들을 조회한다.
```

```sql
- 셀프 조인
-- 동일한 한 테이블에서 조인하는 방법
SELECT
a.employee_id
, a.emp_name
, b.employee_id
, b.emp_name
, a.department_id
FROM
employees a
, employees b
WHERE a.employee_id < b.employee_id
AND a.department_id = b.department_id
AND a.department_id = 20;
```

```sql
- OUTER JOIN
-- 조인 조건에 만족하지 않더라도 데이터를 모두 추출함
-- 무조건 ID가 매칭이 된 것만 조회
SELECT
a.department_id
, a.department_name
, b.job_id
, b.department_id
FROM
departments a
, job_history b
WHERE a.department_id = b.department_id;
```

```sql
- 매칭이 안되는 칼럼은 null로 출력
SELECT
a.department_id
, a.department_name
, b.job_id
, b.department_id
FROM
departments a
, job_history b
WHERE a.department_id = b.department_id(+); -- (+) : 외부조인 = null값도 나옴
```

```sql
-
SELECT
a.employee_id
, a.emp_name
, b.job_id
, b.department_id
FROM
employees a
, job_history b
WHERE a.employee_id = b.employee_id(+)
AND a.department_id = b.department_id(+); -- null 값이 나오려면 여기서 외부조인이 있어야함
```

```sql
- 카타시안 조인
-- 사원 테이블의 총 건수는 107건
-- 부서 테이블의 총 건수는 27건
-- 107 x 27 = 2,889건
```

```sql
- ANSI 조인
-- ANSI SQL 문법 (JOIN 명 들어감)
```

```sql
- 2013년 1월 1일 이후에 입사한 사원번호, 사원명, 부서번호, 부서명을 조회하는 쿼리 비교
-- 기존 문법
SELECT
a.employee_id
, a.emp_name
, a.hire_date
, b.department_id
, b.department_name
FROM
employees a
, departments b
WHERE a.department_id = b.department_id
AND a.hire_date >= TO_DATE('2003-01-01', 'YYYY-MM-DD');
```

```sql
- ANSI
SELECT
a.employee_id
, a.emp_name
, a.hire_date
, b.department_id
, b.department_name
FROM
employees a
INNER JOIN departments b
ON (a.department_id = b.department_id)
WHERE a.department_id = b.department_id
AND a.hire_date >= TO_DATE('2003-01-01', 'YYYY-MM-DD');
```

```sql
- 서브쿼리
-- SQL 문장 안에서 보조로 사용되는 또 다른 SELECT 문을 의미
-- 구조 : (1) 메인 쿼리 / (2) 서브 쿼리
SELECT (서브쿼리 들어갈 수 O
SELECT (서브쿼리) FROM WHERE GROUP BY HAVING ORDER BY)
FROM (서브쿼리 들어갈 수 O
SELECT FROM WHERE GROUP BY HAVING ORDER BY)
WHERE (서브쿼리 들어갈 수 O
SELECT FROM WHERE GROUP BY HAVING ORDER BY)
GROUP BY
HAVING
ORDER BY;
```

```sql
- 연관성이 없는 서브 쿼리
SELECT * FROM employees; -- 107개의 행
```

```sql
- 메인 쿼리 : 모든 사원 테이블을 조회
-- 서브 쿼리 : 조건 - 사원테이블의 평균 급여보다 많은 사원
-- 결괏값은 51개
SELECT AVG(salary) FROM employees;
```

```sql
SELECT *
FROM employees
WHERE salary >= (SELECT AVG(salary) FROM employees);
```

```sql
- parent_id가 NULL인 부서번호를 가진 총 사원의 건수
SELECT department_id
FROM departments
WHERE parent_id IS NULL;
```

```sql
SELECT count(*)
FROM employees
WHERE department_id IN (SELECT department_id
FROM departments
WHERE parent_id IS NULL);
```

```sql
SELECT employee_id , job_id FROM job_history;
```

```sql
SELECT
employee_id
, emp_name
, job_id
FROM employees
WHERE (employee_id, job_id) IN (SELECT
employee_id
, job_id
FROM job_history);
```

```sql
- 연관성 있는 서브 쿼리
SELECT 1
FROM
dapartment a
, job_history b
WHERE a.department_id = b.department_id
```

```sql
SELECT
a.department_id
, a.department_name
FROM departments a
WHERE EXISTS (SELECT 1
FROM job_history b
WHERE a.department_id = b.department_id);
```

```sql
- SELECT 절에 서브쿼리가 존재하는 케이스
SELECT
a.employee_id
, (SELECT b.emp_name
FROM employees b
WHERE a.employee_id = b.employee_id) AS emp_name
, a.department_id
, (SELECT b.department_name
FROM departments b
WHERE a.department_id = b.department_id) AS dep_name
, (SELECT c.job_title
FROM jobs c
WHERE a.job_id = c.job_id) AS title_name
FROM job_history a;
```

```sql
- 중첩 서브쿼리
SELECT
a.department_id
, a.department_name
FROM departments a
WHERE EXISTS (SELECT 1
FROM employees b
WHERE a.department_id = b.department_id
AND b.salary > (SELECT AVG(salary) FROM employees)
);
```

