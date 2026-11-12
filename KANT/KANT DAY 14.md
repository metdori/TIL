# KANT DAY 14

## DOCKER
- image = 게임칩
- container = 실행한 게임

### 도커 기본 사이클
```bash
터미널
docker --version # 도커 버전 확인
docker compose version # 여러 컨테이너 정의하는 docker compose 버전 확인
docker info # 도커 상세 정보 확인
docker pull hello-world # 도커 허브에서 해당 이미지를 다운
docker run hello-world # 해당 이미지로 컨테이너를 생성하고 실행
docker ps -a # ps = process status 실행중인 컨테이너만 보여줌, -a = all 종료된 컨테이너까지 전부 보여줌
# docker 앱의 containers 항목에 생성된 걸 확인할 수 있음
docker rmi hello-world # 아직 컨테이너를 안 지워서 오류가 뜸, 순서: 컨테이너 종료->컨테이너 삭제->이미지 삭제
docker rmi hello-world -f # 강제로 이미지 태그를 삭제 (이미지 데이터는 컨테이너 안에 유지)
docker images # 이미지 목록 조회, hello-world 이미지가 삭제되어있음
docker ps -a # hello-world 컨테이너는 남아있음
docker stop (CONTAINER ID) # 해당 컨테이너 정지
docker rm (CONTAINER ID) # 해당 컨테이너 삭제
docker ps -a # 해당 지워져있음
```

### Ubuntu 컨테이너와 대화형 터미널 (interactive terminal)
```bash
docker pull ubuntu:24.04 # 도커 허브에서 해당 이미지를 다운
docker run -it --name my-server ubuntu:24.04 bash
# -it = interactive terminal 터미널을 열고 키보드로 명령어를 칠 수 있게 만드는 세트, --name = 터미널 이름 설정, bash = 컨테이너 기본셸 bash로 지정

cd etc # /etc 에는 리눅스 설정 파일들이 있음
ls -al
cat os-release # cat = concatenate 연관시키다, os-release 파일 안 내용을 조회, docker 앱 Exec 항목에서 해도 됨
apt update # apt 패키지 관리 도구를 이용하여 소프트웨어 최신 업데이트
exit #  현재 실행 중인 셸 종료하고 컨테이너도 정지
docker ps # 정지돼서 ubuntu 안 떠있음
docker ps -a # ubuntu 떠있음
docker start -ai my-server # 정지된 컨테이너 실행, -ai = 멈춰 있던 컨테이너를 켜면서 화면 출력과 키보드를 내 터미널에 연결 
echo "# Hello Docker" > README.md
ls # 컨테이너 내부에 README.md 파일 생성됨
exit
docker ps -a # 컨테이너 종료됨
docker rm (CONTAINER ID)
docker ps -a # 컨테이너 삭제됨
```

### Nginx 웹서버 구동 및 포트 포워딩 (Port Forwarding)
```bash
docker pull nginx # 엔진엑스 웹서버 프로그램 이미지를 다운
docker images # nginx 생김
docker run -d --name my-webserver -p 8080:80 nginx # -d = detach 백그라운드에서 실행, -p = port forwarding 내 PC 8080 포트랑 웹서버 80 포트 연결
docker ps # my-webserver 생김
curl http://www.google.co.kr # 특정 웹사이트에 HTTP 요청을 보내 응답을 확인
curl localhost:8080
netstat -ano | grep 8080
# netstat = network statistics 네트워크 연결 확인, -a 모든소켓, -n 숫자주소표시, -o PID 프로세스 id표시, | 앞의 결과 뒤로 전달, grep 8080 만 필터링
docker exec -it my-webserver bash # exec 실행 중인 컨테이너에 명령추가, bash = 컨테이너 안에서 실행할 기본 리눅스 셸 프로그램 지정
cd /usr/share/ngnix/html # 웹서버 기본 웹페이지 파일이 있는 기본 경로
cat index.html # 현재 웹서버의 html 코드를 조회
echo "<h1>Hello Docker<h/1>" > index.html # 해당 내용으로 html 내용 수정
exit
docker logs -f my-webserver # 컨테이너에서 출력되는 로그를 출력, -f = follow 로그를 실시간 스트리밍
Ctrl + C # 로그 실시간 스트리밍 종료
```

### MYSQL 데이터베이스 컨테이너
```bash
docker pull mysql:8.4 # 오픈소스 관계형 데이터베이스 이미지 다운

docker run -d --name my-sql -e MYSQL_ROOT_PASSWORD=admin1234 -p 3306:3306 mysql:8.4 # -e = environment 환경변수 설정, ROOT = MYSQL 최고 관리자
docker ps
docker logs -f my-sql
docker exec -it my-sql mysql -uroot -p # mysql = 컨테이너 안에서 mysql 실행 프로그램 구동, -u = user, -p = 비밀번호 입력 프롬프트 요구
admin1234 # 원래 비밀번호 입력해도 안보임
SHOW DATABASES; # show 는 전부 대문자로 입력, 데이터베이스 목록 조회
CREATE DATABASE shop; # shop 데이터베이스 생성
SHOW DATABASES; # shop 생성됨
docker ps
exit
docker stop my-sql
docker rm my-sql
```

### POSTGRESQL 데이터베이스 컨테이너
```bash
docker pull postgres:18 # 오픈소스 데이터베이스 이미지 다운
docker run -d --name my-pg -e POSTGRES_PASSWORD=admin1234 -p 5432:5432 postgres:18
docker logs my-pg
docker exec -it my-pg psql -U postgres # psql = PostgreSQL 전용 터미널 실행 명령어, -U postgres = 슈퍼계정 postgres 로 접속

\l # list
\dt # display table = 데이터베이스 안의 테이블 표시
\q # quit
```








