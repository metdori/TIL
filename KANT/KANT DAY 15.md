# KANT DAY 15

## PostgreSQL 환경 컨테이너

### postgreSQL 컨테이너 생성
```bash
mkdir 02_database
cd 02_database
mkdir pg_lab
cd pg_lab

docker run -d --name my-pg -e POSTGRES_PASSWORD=admin1234 -p 5432:5432 postgres:18 # postgres:18버전으로 my-pg 컨테이너 실행
# -d = detached 백그라운드 실행 (안하면 터미널에 실시간 로그), -e = environment 환경변수 설정 (무조건 대문자), -p = port 포트 연결, 5432 = POSTGRES 표준포트, postgres:18 = 다운할 이미지 이름, 버전
docker ps # 컨테이너 백그라운드에서 생성됨
docker stop my-pg # 컨테이너 정지
docker start my-pg # 컨테이너 실행

docker exec -it my-pg psql -U postgres # postgres 로 컨테이너 접속, exec = 실행 중인 컨테이너 안에서 명령어 실행, -it(interactive terminal) = 가상 터미널 환경, psql = PostgresSQL 전용 터미널 프로그램,
-U = username, postgres = 최고 관리자 계정 (-it 없을 경우 터미널 없이 백그라운드에서 psql 만 실행. 뒤에 -c, -f 같은 옵션 바로 전달할 경우엔 -it 없어도 됨)
```
```sql
CREATE DATABASE kant_shop; # 데이터베이스 = 데이터를 체계적으로 저장, 관리하는 시스템, 데이터를 표(table) 형식으로 저장

\l # list 데이터베이스 리스트 조회
DROP DATABASE kant_shop; # kant_shop DB 삭제
exit #  DB 터미널 종료
```

- vscode Docker 확장프로그램 설치

### 데이터베이스 생성
```bash
touch kant_shop.sql
code kant_shop.sql # kant_shop.sql 파일 바로 열기
```

```sql
-- 1. 데이터베이스 생성 # -- = SQL 표준 주석처리

DROP DATABASE IF EXISTS kant_shop; # 이미 존재할 경우 기존 DB 삭제

CREATE DATABASE kant_shop;

-- 2. 특정 데이터베이스에 접속 # c = connect
\c kant_shop

-- 3. 상품 테이블 생성
CREATE TABLE product # product 라는 테이블 생성, () = 테이블 열 제목들
(
    id SERIAL PRIMARY KEY, # id 열 제목, SERIAL = 숫자 1부터 자동 부여, PRIMARY KEY = 숫자를 각 줄의 대표 번호로 지정
    name VARCHAR(100) NOT NULL, # name 열 제목,  VARCHAR(100) 최대 100글자 문자열 자료형, NOT NULL = 공백 불가능
    price INT NOT NULL, # price 열 제목, INT = integer 정수
    stock INT NOT NULL
);

-- 4. 상품 초기 데이터 입력
INSERT INTO product (name, price, stock) VALUES # id 어차피 SERIAL 자동 부여, VALUES = 실제값들
('무선 키보드', 35000, 120), # SQL 에서 문자값은 무조건 ''
('게이밍 마우스', 28000, 85),
('27인치 모니터', 259000, 15),
('USB-C 허브', 19900, 200),
('노트북 거치대', 24000, 7);
```

```bash
touch docker_cmd.md
docker cp kant_shop.sql my-pg:/tmp/kant_shop.sql # cp 대상파일 복제파일, my-pg 컨테이너 내부에 파일 복사, Vscode 내의 Containers 항목에서 복제된 거 확인 가능
docker exec -it my-pg psql -U postgres -f //tmp/kant_shop.sql # -f = file 파일을 실행, // 말고 / 쓰려면 Bash셸에 리눅스 환경변수 등록해야 함
```

### Bash셸에 리눅스 환경변수 등록
```bash
echo 'export MSYS_NO_PATHCONV=1' >> ~/.bashrc # MSYS(Git Bash 엔진)에 경로변환 금지값을 1(True)로 설정, ~/.bashrc =  터미널 환경변수 파일
source ~/.bashrc # source = 지정한 파일을 현재 터미널에서 즉시 실행
```

```bash
docker exec -it my-pg psql -U postgres -d kant_shop # -d = database 특정 컨테이너에서 내부 특정 DB 바로 실행
SELECT * FROM product; # SELECT = 조회 SQL 쿼리문, * = 전부 SQL 쿼리문, FROM = 무엇을, ; = SQL 명령어 종료

docker exec -i my-pg psql -U postgres < kant_shop.sql # 내PC의 해당 파일 내용을 컨테이너에 전달해서 실행 (-t 쓰면 안됨) 

SELECT name, price FROM product; # name, price 값만 조회
\q # DB 종료
```

### 도커 볼륨 생성
```bash
docker volume ls # volume = 컨테이너 외부의 데이터 저장공간
docker volume create pgdata
```

### 도커 COMPOSE 사용
```bash
touch compose.yaml # compose = 여러 docker 명령어를 한 번에 실행
cd compose.yaml # yaml = 사람이 읽기 쉬운 파일 형식
```

```bash
# yaml 형식은 Tab 불가능, Enter + Space 사용
services:                 # 실행할 컨테이너(서비스) 목록 — 필수
  서비스이름:              # 예: db, web (직접 정하는 이름)
    image: 이미지:태그      # 사용할 이미지
    container_name: 이름   # 컨테이너 이름 (생략 시 자동 생성)
    environment:           # 환경 변수 (-e)
      KEY: value
    ports:                 # 포트 연결 (-p) — 문자열로 감싸는 것을 권장
      - "호스트:컨테이너"
    volumes:               # 볼륨 연결 (-v)
      - 볼륨이름:컨테이너경로

volumes:                  # 사용할 볼륨 선언 (최상위)
  볼륨이름:

```bash
services:                 # 실행할 컨테이너(서비스) 목록 — 필수
  db:              # 예: db, web (직접 정하는 이름)
    image: postgres:18      # 사용할 이미지
    container_name: kant-pg   # 컨테이너 이름 (생략 시 자동 생성)
    environment:           # 환경 변수 (-e)
      POSTGRES_PASSWORD: admin1234
    ports:                 # 포트 연결 (-p) — 문자열로 감싸는 것을 권장
      - "5432:5432"
    volumes:               # 볼륨 연결 (-v)
      - pgdata:/var/lib/postgresql

volumes:                  # 사용할 볼륨 선언 (최상위)
  pgdata:
    external: true
```

### COMPOSE 명령어
```
docker compose config # compose 설정 확인
docker compose version # compose 버전 확인
docker compose up -d # -d = detached, 백그라운드에서 compose 실행, 일반적으로 compose.yaml 파일 실행, 파일 없으면 docker compose -f 파일명.yaml up -d
docker compose down # 컨테이너, 네트워크 삭제
docker compose down -v # 컨테이너, 네트워크 + 볼륨까지 삭제

docker compose up -d
docker exec -it kant-pg psql -U postgres -d kant_shop -c "SELECT * FROM product" # -d = database, -c = command
docker compose ps # compose 가 관리하는 컨테이너들 상태 확인
```

- Ctrl Shift X = vscode 확장프로그램 탭 단축키
- Database 확장프로그램 설치













