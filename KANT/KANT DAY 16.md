# KANT DAY 16

## 데이터베이스와 SQL 기초

- 데이터(Data) = 사실을 표현하는 원시 값, 정보
- 데이터베이스(Database) = 체계적으로 정리된 데이터의 집합
- 쿼리(Query) = 데이터베이스에 보내는 명령
- SQL = 쿼리를 쓰는 표준 언어
___
## DBMS(DataBase Management System) = 데이터베이스 관리 소프트웨어
- MySQL = 오픈소스, 가볍고 빠름 / 웹 서비스, 스타트업
- PostgreSQL = 오픈소스, 기능이 풍부하고 안정적 / 데이터 분석, 대기업
- Oracle = 상용, 대용량 처리에 강함 / 금융, 공공기관
- SQL Server = 마이크로소프트, Windows 환경 / 기업 환경
- SQLite = 초경량, 파일 기반 / 모바일, 임베디드
- MariaDB = MySQL 에서 파생된 오픈소스 / 웹 서비스

## 데이터 저장 모델
- 계층형(HDBMS) = Hierarchical - 트리(부모-자식) - 상하 관계가 명확하지만 유연성이 낮음 ex. IBM IMS
- 관계형(RDBMS) = Relational - 테이블(행-열) - 유연하고 SQL로 조회 ex. MySQL PostgreSQL
- 망형(NDBMS) = Network - 네트워크(多대多) - 복잡한 관계 표현 가능, 구현이 어려움 ex. IDS

관계형이 표준인 이유: 표 형태라 직관적, 복잡한 여러 테이블 연결 가능, 무결성 보장, 트랜잭션, 성능 최적화 

## SQL 작성 순서
```sql
SELECT region, COUNT(*) AS cnt
FROM members
WHERE birth_year >= 1980
GROUP BY region
HAVING COUNT(*) >= 3
ORDER BY cnt DESC
```


## 도커

```bash
cd docker-compose.yaml # yaml 파일 = compose 가 컨테이너를 만들 때 읽는 설계도
```
```bash
name: sportscenter

services:
  db:
    container_name: sportscenter-db
    image: postgres:18
    environment:
      POSTGRES_PASSWORD: admin1234
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql

volumes:
  pgdata:
```
```bash
docker compose up -d # -d = detached 백그라운드에서 실행, 해당 설계도대로 Cotainer, Volume 생성됨
```
- Vscode->Containers->Volumes->Inspect 누르면 해당 Volume 상세 정보 조회 가능
```bash
docker compose exec db psql -U postgres # compose 로 만든 db 서비스의 컨테이너에 psql 실행하고 postgres 사용자로 접속
```
- compose = yaml 파일에 적힌 컨테이너 실행 설정들을 명령 하나로 만드는 Docker 도구
- yaml = compose 가 컨테이너를 만들 때 읽는 설계도

## 프로젝트 실습

### 데이터베이스 생성
```sql
CREATE DATABASE sportscenter; # 해당 데이터베이스 생성
\l
\c sportscenter # c = connect, 해당 데이터베이스 연결
```
- DBcode 도구에서 SQL 명령어로 생성한 데이터베이스 연결 가능
- Schemas = DB 안에서 데이블을 담는 폴더, 기본적으로 Schemas/Public 폴더 안에 담겨짐
```bash
docker compose stop # DB 가 용량을 많이 차지하므로 사용 안 할 때는 stop = 종료, down = 삭제
```

### 회원 테이블
```bash
docker compose exec db psql -U postgres -d sportscenter
```
```sql
CREATE TABLE members
(
    member_id   SERIAL      PRIMARY KEY,
    name        VARCHAR(30) NOT NULL, # VARCHAR = 가변 길이 문자열, 최대 30자
    gender      CHAR(1)     NOT NULL, # CHAR = 고정 길이 문자열, 무조건 1자
    birth_year  SMALLINT    NOT NULL, # SMALLINT = 2바이트 크기의 정수, -32,768~32,767까지
    phone       VARCHAR(20),
    email       VARCHAR(50),
    region      VARCHAR(10),
    join_date   DATE        NOT NULL # DATE = 년-월-일 YYYY-MM-DD
);
```
```sql
\dt # d = describe, 테이블 조회
\d members # members 테이블의 상세 정보 조회
```
- SERIAL 명령어를 실행해주는 id_seq 객체가 생성됨 (schmas/public/sequences/members_member_id_seq)

### 회원 등록
- 여러가지 INSERT 방법
```sql
- INSERT INTO members (컬럼1, 컬럼2, 컬럼3) VALUES (컬럼1값, 컬럼2값, 컬럼3값);
  
- INSERT INTO members VALUES (모든 컬럼, , , , );
  
- INSERT INTO members SELECT * FROM WHERE ~~~;
  
- INSERT INTO members (컬럼1, 컬럼2, 컬럼3)
  VALUES 
 (컬럼1값, 컬럼2값, 컬럼3값),
 (컬럼1값, 컬럼2값, 컬럼3값),
 (컬럼1값, 컬럼2값, 컬럼3값),
 (컬럼1값, 컬럼2값, 컬럼3값);
```
```sql
INSERT INTO members 
(
    name, 
    gender, 
    birth_year,
    phone,
    email,
    region, 
    join_date
)
VALUES
(
    '김하늘', 
    'F', 
    1995, 
    '010-1234-5678', 
    'haneul@example.com', 
    '마포구', 
    '2026-09-01'
);
```
```sql
SELECT * FROM members; # members 테이블에서 모두 조회
SELECT name, region FROM public.members; # members 테이블에서 name, region 조회(public 생략 가능)

INSERT INTO members (member_id, name, gender, birth_year, join_date)
VALUES (1, '홍길동', 'M', 2010, '2025-01-01'); # 입력하 member_id 가 PRIMARY KEY 라서 오류가 남
```
- Ctrl + U = 입력한 명령어 지우기
- Ctrl + L = 터미널 내역들 지우기

### 종목 개설강좌 목록
```sql
CREATE TABLE sports
(
    sports_id   SERIAL      PRIMARY KEY,
    sport_name  VARCHAR(20) NOT NULL 
);

INSERT INTO sports (sport_name) VALUES ('수영');
INSERT INTO sports (sport_name) VALUES ('헬스');
```

### 개설 강좌
```sql
CREATE TABLE courses
(
    course_id       SERIAL      PRIMARY KEY,
    sports_id       INT         NOT NULL REFERENCES sports (sports_id), # REFERENCES = 외래키 ()값들만 포함
    course_name     VARCHAR(30) NOT NULL,
    level           VARCHAR(10) NOT NULL,
    class_days      VARCHAR(10) NOT NULL,
    start_time      TIME        NOT NULL,
    capacity        SMALLINT    NOT NULL,
    fee             INT         NOT NULL
);

INSERT INTO courses (sports_id, course_name, level, class_days, start_time, capacity, fee)
VALUES (1, '수영 초급 새벽반', '초급', '월수금', '06:00', 20, 70000);
INSERT INTO courses (sports_id, course_name, level, class_days, start_time, capacity, fee)
VALUES (2, '헬스 자유이용', '자유', '매일', '06:00', 50, 50000);

INSERT INTO courses (sports_id, course_name, level, class_days, start_time, capacity, fee)
VALUES (30, '헬스 자유이용', '자유', '매일', '06:00', 50, 50000);

DROP TABLE IF EXISTS lockers;
```

### 1:1 사물함 테이블
```
CREATE TABLE lockers
(
    locker_no   INT     PRIMARY KEY,
    member_id   INT     NOT NULL UNIQUE REFERENCES members (member_id) ON DELETE CASCADE, # ON DELETE = 부모 행이 삭제가 될 때 ~해라, CASCADE = 폭포처럼 흐르다(자식 행)
    asign_date  DATE    NOT NULL
);

INSERT INTO lockers VALUES (101, 1, '2026-11-01');
```

```sql
-- 삭제 테스트를 위한 임시 회원 추가
INSERT INTO members (name, gender, birth_year, join_date)
VALUES ('임시회원', 'M', 2000, '2026-11-01');

-- 임시 회원의 락커 배정
INSERT INTO lockers VALUES (102, 3, '2026-11-01');

-- 회원 삭제 시 사물함도 함께 삭제되는지 확인
DELETE FROM members WHERE member_id = 3;

-- 사물함이 삭제되었는지 확인
SELECT * FROM lockers;
```

### 수강 신청 테이블
```sql
CREATE TABLE enrollments
(
    member_id       INT         NOT NULL REFERENCES members (member_id),
    course_id       INT         NOT NULL REFERENCES courses (course_id),
    enroll_month    CHAR(7)     NOT NULL,
    status          VARCHAR(10) NOT NULL,
    pay_amount      NUMERIC(10,2)    NOT NULL CHECK (pay_amount >=0),
    PRIMARY KEY (member_id, course_id, enroll_month)
);

INSERT INTO enrollments
VALUES (1, 1, '2026-11', '수료', 50000);

INSERT INTO enrollments
VALUES (2, 2, '2026-10', '수료', 70000);

INSERT INTO enrollments
VALUES (2, 2, '2026-10', '수료', 60000);
```

### 강사 테이블
```sql
DROP TABLE IF EXISTS instructor CASCADE;

CREATE TABLE instructors
(
    instructor_id       SERIAL          PRIMARY KEY,
    name                VARCHAR(20)     NOT NULL,
    sports_id            INT             NOT NULL REFERENCES  sports (sports_id),
    phone               VARCHAR(20),        
    hire_date           DATE            NOT NULL
);

INSERT INTO instructors (name, sports_id, phone, hire_date)
VALUES
('홍길동', 1, '010-1234-1234', '2025-01-01'),
('손흥민', 2, '010-1234-8080', '2025-01-10');
```

## 테이블 수정 = ALTER TABLE
```sql
ALTER TABLE courses # ALTER = 바꾸다, 테이블 수정
ADD COLUMN instructor_id INT REFERENCES instructor (instructor_id);
```

```sql
SELECT
course_name AS 강좌명, # 테이블의 course_name 값을 강좌명으로 지칭
fee AS 수강료,
fee * 3 AS "3개월 수강료"
FROM courses
LIMIT 5;

SELECT DISTINCT region FROM members; # DISTINCT = 별개의(중복된 값 제거)

SELECT DISTINCT sport_id, level FROM courses;

SELECT * FROM members WHERE member_id <= 7;

SELECT * FROM members WHERE region = '마포구';

SELECT COUNT(*) AS "E-MAIL 미기재" FROM members WHERE email is NULL;

SELECT * FROM members WHERE birth_year BETWEEN 1995 AND 2000;

SELECT * FROM courses WHERE course_id IN (1, 5, 10);

SELECT * FROM members WHERE name LIKE '%'; # % = 와일드카드, 모든 값이든 가능
```

