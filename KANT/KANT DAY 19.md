# KANT DAY 19

- DBcode 에서 원하는 DB 내에 Open Query File 로 DB 연결된 파일 바로 생성 가능
- Ctrl + Enter 로 DB 연결하면 SQL 실행 결과 바로 조회 가능

## SQL 문법
---
- RETURNING = 방금 처리한 행의 결과를 바로 출력
```sql
INSERT INTO sports (sport_name)
VALUES ('필라테스')
RETURNING *;
```

- TRUNCATE - 데이터만 삭제, 테이블은 유지, WHERE 불가능, 빠름
```sql
TRUNCATE contact_lists;
```

- SELECT 임의의 값 AS 이름표 / members 테이블에서 email IS NULL 에 해당하는 값을 '이메일 미등록' 이라는 reason 값으로 지정
```sql
INSERT INTO contact_lists (member_id, name, reason, request_date)
SELECT member_id, name, '이메일 미등록' AS reason, '2026-11-19' AS req_date
FROM members
WHERE email IS NULL
RETURNING *;
```

- UPDATE = 테이블 데이터 바꾸기, SET = 어떤 칼럼을 바꿀지 설정
```sql
UPDATE members
SET email = 'ljh@abc.com'
WHERE member_id = 2
RETURNING *;
```

## 서브쿼리 활용한 UPDATE
- IN = 여러 값 중 일치하는 값을 확인하는 연산자
```sql
-- 2026-11 수강신청인원이 5명 이상인 경우 수강신청인원수 +5
UPDATE courses
SET capacity = capacity + 5
WHERE course_id IN (SELECT course_id
                    FROM enrollments
                    WHERE enroll_month = '2026-11'
                    GROUP BY course_id
                    HAVING COUNT(*) >= 5)
RETURNING course_id, course_name, capacity;
```

- DELETE - 데이터만 삭제, 테이블은 유지, WHERE 가능, 느림
```
SELECT * FROM lockers;

DELETE FROM lockers
WHERE member_id = 38
RETURNING * ;
```

- SELECT 1 = 숫자 1 을 선택, 만약 1 이란 값이 없을 경우 False, 즉 값의 존재 여부를 판별
```sql
SELECT m.member_id, m.name, m.join_date
FROM members m
WHERE NOT EXISTS (SELECT 1 FROM enrollments e WHERE e.member_id = m.member_id);

DELETE FROM members m
WHERE NOT EXISTS (SELECT 1 FROM enrollments e WHERE e.member_id = m.member_id)
RETURNING m.member_id, m.name;
```


- DROP - 데이터, 테이블까지 삭제
- DELETE - 데이터만 삭제, 테이블은 유지, WHERE 가능, 느림
- TRUNCATE - 데이터만 삭제, 테이블은 유지, WHERE 불가능, 빠름

## psycopg = Python 에서 PostgreSQL DB 에 접속하기 위한 라이브러리
```python
uv init --no-package
uv sync
git init
git add -A
uv add "psycopg[binary]" # binary = 미리 컴파일된 바이너리 패키지로 설치
```


