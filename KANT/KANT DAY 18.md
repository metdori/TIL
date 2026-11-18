# KANT DAY 18


## INSERT문 종류

- 원하는 칼럼만 골라서 1줄 넣기
```sql
INSERT INTO members (컬럼1, 컬럼2, 컬럼3) VALUES (컬럼1값, 컬럼2값, 컬럼3값);
```
- 컬럼 목록 생략하고 1줄 넣기 (테이블 설계 순서와 정확히 같아야 함)
```sql
INSERT INTO members VALUES (컬럼1값, 컬럼2값, 컬럼3값, ~);
```
- 다른 테이블의 조회 결과를 복사해서 넣기
```sql
INSERT INTO members SELECT * FROM 다른테이블 WHERE 조건;
```
- 여러 줄 한꺼번에 넣기
```sql
INSERT INTO members (컬럼1, 컬럼2, 컬럼3)
VALUES
(컬럼1값, 컬럼2값, 컬럼3값),
(컬럼1값, 컬럼2값, 컬럼3값),
(컬럼1값, 컬럼2값, 컬럼3값);
```
---
## SQL 문법 정리

- REFERENCES = sports 테이블의 sports_id 값(외래키)을 참조하여 sp_id 생성, 무조건 외래키값만이 sp_id 안에 들어갈 수 있음
```sql
sp_id  INT  NOT NULL  REFERENCES sports (sports_id)
```
- UNIQUE = 중복 불가, ON DELETE CASCADE = 삭제될 경우 자식행도 전부 삭제
```sql
CREATE TABLE lockers
(
    locker_no   INT     PRIMARY KEY,
    member_id   INT     NOT NULL UNIQUE REFERENCES members (member_id) ON DELETE CASCADE,
    asign_date  DATE    NOT NULL
);
```
- NUMERIC(10,2) = 최대 10자리이고 소수점 2자리까지 표시 ex. 12345678.00, CHECK = 테이블 생성 조건, WHERE = 테이블 조회 조건
- PRIMARY KEY (member_id, course_id, enroll_month) = 해당 데이터 3개가 전부 중복되는 값은 불가능
```sql
CREATE TABLE enrollments
(
    member_id       INT         NOT NULL REFERENCES members (member_id),
    course_id       INT         NOT NULL REFERENCES courses (course_id),
    enroll_month    CHAR(7)     NOT NULL,
    status          VARCHAR(10) NOT NULL,
    pay_amount      NUMERIC(10,2)    NOT NULL CHECK (pay_amount >=0),
    PRIMARY KEY (member_id, course_id, enroll_month)
);
```
- DISTINCT = 중복 불가능 조건
```sql
SELECT DISTINCT region FROM members;
```
- CASE WHEN THEN ELSE END AS = 해당하는 조건에 이름표 적용
```sql
SELECT name,  birth_year,
        CASE WHEN birth_year <= 1960 THEN '할인'
            WHEN birth_year >= 2000 THEN '청소년'
            ELSE '일반'
        END AS 요금구분
FROM members
ORDER BY birth_year, name
LIMIT 10;
```
- ORDER BY = 기본적으로 오름차순 정렬, DESC 내림차순 ASC 오름차순, NULL 값은 무조건 가장 큰 값으로 인식
```sql
SELECT name, email
FROM members
ORDER BY email DESC, name; # NULL 가장 큰 값으로 인식
```
- ROUND(값, 2) = 값을 소수점 2번째자리에서 반올림, AVG = 평균
```sql
SELECT
    COUNT(*)        AS 강좌수,
    MIN(fee)        AS 최저수강료,
    MAX(fee)        AS 최고수강료,
    SUM(capacity)   AS 전체정원,
    ROUND(AVG(fee), 2)        AS 평균수강료
FROM courses;
```
- <> = ~가 아니다 뜻, HAVING = 그룹데이터 조건 / WHERE = 개인데이터 조건
```sql
SELECT course_id, count(*) AS 누적신청
FROM enrollments
WHERE status <> '취소'
GROUP BY course_id
HAVING COUNT(*) >= 18
ORDER BY 누적신청 DESC, course_id;
```
- UNION = 중복 제거하고 합치기 / UNION ALL = 중복 상관없이 합치기
```sql
SELECT member_id FROM enrollments WHERE course_id = 4 AND enroll_month = '2026-09'
UNION
SELECT member_id FROM enrollments WHERE course_id = 4 AND enroll_month = '2026-10'
UNION
SELECT member_id FROM enrollments WHERE course_id = 4 AND enroll_month = '2026-11'
ORDER BY member_id;

SELECT '강사' AS 구분, name, phone FROM instructors WHERE sport_id = 4
UNION ALL
SELECT '회원' AS 구분, name, phone FROM members WHERE region = '영등포구';
```
- INNER JOIN LEFT JOIN RIGHT JOIN FULL (OUTER) JOIN / FROM A LEFT JOIN B 에서 A 가 왼쪽, B 가 오른쪽
```sql
SELECT
    e.member_id,
    m.name,
    c.course_name,
    e.enroll_month,
    e.status
FROM enrollments e
LEFT JOIN members m ON e.member_id = m.member_id
LEFT JOIN courses c ON e.course_id = c.course_id
WHERE e.course_id = 1 AND e.enroll_month = '2026-11'
ORDER BY m.name;
```
- COLLATE = 수집:분석하다, 데이터를 정렬할 때 규칙 / ko-x-icu = 한국어 유니코드 정렬기준
```sql
ORDER BY region COLLATE "ko-x-icu";
```
---
- /t = 튜블(데이터)만 출력하는 모드





