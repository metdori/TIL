# KANT DAY 17


## DDL (Data Definition Language, 데이터 정의어)
- 데이터베이스의 구조나 뼈대를 정의하고 관리하는 명령어
- 테이블을 생성하거나, 수정하고, 삭제
- CREATE(생성), ALTER(수정), DROP(삭제), TRUNCATE(비우기)

## DML (Data Manipulation Language, 데이터 조작어)
- 테이블 안에 들어있는 실제 데이터를 조회, 입력, 수정, 삭제하는 명령어
- SELECT(조회), INSERT(입력), UPDATE(수정), DELETE(삭제)

  

- pay_amount NUMERIC(10, 2) # NUMERIC 고정 소수점 숫자, 최대 10자리 중에서 소숫점은 최대 2자리
- username VARCHAR(50) # VARCHAR 가변 길이 문자열
- user_id INT # INT 소수점이 없는 정수
- CONSTRAINT chk_age CHECK (age >= 18) # CONSTRAINT 제약 조건
- TRUNCATE TABLE access_logs; # TRUNCATE 데이터 일괄 삭제 명령어, 뼈대는 삭제 X / DROP 뼈대까지 삭제 O


```sql
-- docker compose cp sportscenter_seed.sql db:/tmp/sportscenter_seed.sql # sql 파일을 컨테이너 안에 복사
-- docker compose exec db psql -U postgres # 컨테이너에 psql 접속
-- \i /tmp/sportscenter_seed.sql # \i = include 지정한 파일 안의 SQL 을 처음부터 끝까지 실행, 복사한 파일 실행

DROP DATABASE sportscenter;

CREATE DATABASE sportscenter;

\c sportscenter

-- 종목 개설강좌 목록
DROP TABLE IF EXISTS sports;

CREATE TABLE sports
(
    sport_id    SERIAL      PRIMARY KEY,
    sport_name  VARCHAR(20) NOT NULL 
);

-- TABLE CREATION
DROP TABLE IF EXISTS members;

CREATE TABLE members
(
    member_id   SERIAL      PRIMARY KEY,
    name        VARCHAR(30) NOT NULL,
    gender      CHAR(1)     NOT NULL,
    birth_year  SMALLINT    NOT NULL,
    phone       VARCHAR(20),
    email       VARCHAR(50),
    region      VARCHAR(10),
    join_date   DATE        NOT NULL
);

-- 강사 테이블
DROP TABLE IF EXISTS instructors;

CREATE TABLE instructors
(
    instructor_id   SERIAL      PRIMARY KEY,
    name            VARCHAR(20) NOT NULL,
    sport_id       INT         NOT NULL REFERENCES sports (sport_id),
    phone           VARCHAR(20),
    hire_date       DATE        NOT NULL
);

-- 개설 강좌
DROP TABLE IF EXISTS courses;

CREATE TABLE courses
(
    course_id       SERIAL      PRIMARY KEY,
    sport_id       INT          NOT NULL REFERENCES sports (sport_id), # REFERENCES 외래키 () 안에 있는 값만 적용
    course_name     VARCHAR(30) NOT NULL,
    level           VARCHAR(10) NOT NULL,
    class_days      VARCHAR(10) NOT NULL,
    start_time      TIME        NOT NULL,
    capacity        SMALLINT    NOT NULL,
    fee             INT         NOT NULL,
    instructor_id   INT         REFERENCES instructors (instructor_id)
);

DROP TABLE IF EXISTS lockers;

-- 1:1 사물함 테이블
CREATE TABLE lockers
(
    locker_no   INT     PRIMARY KEY,
    member_id   INT     NOT NULL UNIQUE REFERENCES members (member_id) ON DELETE CASCADE, # 부모행 member_id 삭제되면 자식행도 전부 삭제
    assign_date  DATE    NOT NULL
);

-- 수강 신청 테이블
DROP TABLE IF EXISTS enrollments;

CREATE TABLE enrollments
(
    member_id       INT             NOT NULL REFERENCES members (member_id),
    course_id       INT             NOT NULL REFERENCES courses (course_id),
    enroll_month    CHAR(7)         NOT NULL,
    status          VARCHAR(10)     NOT NULL,
    pay_amount      NUMERIC(10,2)   DEFAULT 0 CHECK (pay_amount >=0), # 입력값 없으면 0 기본값, 음수이면 에러 발생
    PRIMARY KEY (member_id, course_id, enroll_month)
);
```
