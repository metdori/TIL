KANT DAY 1



winget 으로 Git 이랑 vscode 를 설치함.
winget 으로 다운할 경우 원본 그대로 설치가 가능해서 보안, 관리 측면에서 뛰어남. 제거도 편함 (uninstall)

winget search git (git 파일들을 검색하고 원하는 프로그램의 장치 ID 값으로 다운로드)
winget install -i Git.Git 실행 (-i 명령어의 경우, 자동다운로드하지 않고 원하는 설정값으로 조정가능)
winget search vscode (방향키로 이전에 입력한 명령어로 이동 가능)
winget install Microsoft.VisualStudioCode 실행




Git 란?
분산 버전 관리 시스템 (버전은 파일이 저장된 기록을 의미)

GitHub 란?
Git 원격 저장소를 호스팅하는 웹 서비스

리포지토리 (Repositoy) = Repo = 저장소 = 프로젝트의 변경 이력(커밋)을 저장하는 공간
저장소의 실체는 .git 폴더 안에 존재



Git의 3가지 영역
1. Working Directory = 작업폴더
실제로 파일을 편집하는 곳

2. Staging Area = index
다음 커밋에 담을 변경을 골라 두는 영역. .git/index 파일로 관리되는 논리적 영역

3. Repository = 저장소
커밋(스냅숏) 이력이 저장되는 영역

Working Dir -> (add) Staging -> (commit) Local Repo -> (push) <- (pull) Remote Repo
push = GitHub 업로드, pull = GitHub 다운로드

add = 추가, commit = 기록 ex. git add A.cs , git commit -m "수정본"




git reset = 팀원이 remote repo 에서 안 받아간 경우 사용
git revert = 팀원이 remote repo 에서 받아간 경우 사용

git reset --soft (커밋만 지움) -- mixed (Staging Area 까지 지움) --hard (작업폴더까지 전부 지움)

git revert 커밋에 되돌림값을 추가 -> 업로드한 걸 팀원이 다운받아서 자동으로 오류 수정가능



Branch = 작업 줄기, merge = 작업 줄기 병합





터미널 실행 이후 Git Bash 를 기본값으로 설정
whoami = 사용자명 출력
pwd = print working directory 현재 디렉터리 출력
clear = 명령어기록 삭제
ls -al (ls = 현재 폴더의 파일 목록 출력 list , -a = 숨김파일까지 전부 표시 all , -l = 소유자, 크기 등 상세히 출력 long)

drwxrwxrwx 에서 rwx 1번째=소유자 권한, 2번째=그룹 사용자 권한, 3번째=나머지 사용자 권한 / r = read, w = write, x = execute

ls -ahl (-h = human-readable 용량 단위를 b 말고 kb mb 등으로 읽기 쉽게 변경)

./ 현재 디렉터리
../ 상위 디렉터리


mkdir kant (폴더 생성)
cd kant
ls (폴더 안에 아무것도 없으니까 아무것도 안 뜸)
ls -al (./ 이랑 ../ 자동생성)
cd .. (상위폴더로 이동)
history (지금까지 실행한 명령어 내역)
!숫자 (history 숫자에 해당하는 명령어 바로 실행)
Ctrl+U (입력한 명령어 타이핑 전체 삭제)
Ctrl+W (입력한 명령어 워드 1개 삭제)
cd ~ (~ = 홈디렉터, 틸드 Tilde)
cd kant/ (k만 입력하고 Tab 누르면 명령어 자동완성)
cd - (바로 이전에 작업하던 디렉터리로 이동)
touch README.md (touch = 파일이 없으면 크기 0 파일 생성, 있으면 수정시간을 현재로 변경 / .md = markdown, 웹에서 서식이 적용된 문서로 보이기 때문에 대다수가 사용)
code . (현재 디렉터리를 vscode 에서 실행)
echo HelloWorld (echo = 메아리, 즉 다시 반환)
echo $SHELL ($ = 뒤에 오는 것을 값으로 치환, SHELL = 변수이름 즉 현재 사용중인 변수의 경로 출력) 
echo "Hello World" > README.md (> = overwrite, README 에 뭐가 적혀있든 입력한 값으로 내용이 덮힘)
ls -al ( README.md 용량이 늘어난 것을 확인 가능)
cat README.md (cat 파일 내용 읽어서 출력)
echo "Welcome to KANT" >> README.md (>> = write, 기존 내용 뒤에 추가)
