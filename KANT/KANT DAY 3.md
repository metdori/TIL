# KANT DAY 3

## 1. 프로젝트 생성 및 Git 초기화

```bash
mkdir kant-study
cd kant-study
git init # git 저장소 초기화
rm -rf .git # git 저장소 삭제

echo "# Git Tutorial" > README.md
git status # README.md 빨간색으로 뜸 = git 이 아직 추적하고 있지 않음

# | 파이프 = 앞 명령어의 출력내용을 뒤 명령어로 전달이므로 이렇게 실행하면 오류 뜸
# && = 순차적으로 실행하는 이걸 써야 함
git add . | git commit -m "docs: README.md 추가" 

cd .git # GIT_DIR! 라고 /git 디렉터리에 대한 경고문 뜸
cd ..

git add . # 모든 파일을 staging area에 올리기
git commit -m "docs: README.md 추가" # 커밋 생성

git log --oneline
```

***

## 2. .gitignore 설정 및 테스트

```bash
touch temp.dat
git status # temp.dat 는 git 으로 관리 안 할 예정
code .

touch .gitignore # 파일명 앞에 . 이 붙으면 숨김파일임
```

- `.gitignore` 파일 안에 `*.dat` 적고 저장하면 `*` 때문에 git 이 모든 `.dat` 파일들을 인식 못 함. `git add .` 을 쳐도 포함 안 하게 되지만 `.gitignore` 파일은 포함됨.
- `.gitignore` 파일 안에 `*.tmp`

```bash
touch sample.tmp
git status # ignore 중인 파일들은 안 뜸

git add .
git commit -m "feat: .gitignore 파일 생성"

mkdir Logs
cd Logs
touch daily.log # Logs 폴더 안에 생성
cd ..
git status # Logs/ 폴더 Untrack 중
```

- `.gitignore` 파일 안에 `[Ll]ogs` 추가 (`[]` 안에 문자들 전부 적용 `Logs`, `logs`)

***

## 3. 원격 저장소(Remote) 연결 및 Push

```bash
git remote -v # 아직 추가 안해서 아무것도 안 뜸
git remote add origin https://github.com/metdori/kant-study.git
git remote -v # 추가해서 origin 이란 이름의 주소 등록
git remote add backup https://github.com/metdori/kant-study.git
git remote -v # 추가해서 backup 이란 이름의 주소 등록
git remote remove backup # backup 이란 이름의 주소 삭제
git push origin main # origin 주소에 main 업로드. Ctrl + Click 으로 주소로 바로 이동 가능

git add .
git commit -m "docs: Logs 폴더 제외"
git log --oneline
```

```text
0a88b38 (HEAD -> main) docs: Logs 폴더 제외
5d673be (origin/main) feat: .gitignore 파일 생성
2b03e17 docs: README.md 추가
```
> 초록색 main은 로컬, 빨간색 origin은 원격 / 현재 로컬 위치는 맨 위 Logs 폴더 제외인데 아직 push 안해서 원격은 feat 에 위치하고 있음

```bash
git push origin main
git log --oneline
```

```text
0a88b38 (HEAD -> main, origin/main) docs: Logs 폴더 제외
5d673be feat: .gitignore 파일 생성
2b03e17 docs: README.md 추가
```
> push 했기 때문에 로컬, 원격 모두 맨 위 Logs 폴더 제외에 위치하고 있음

***

## 4. 폴더 삭제 및 Clone 테스트

```bash
cd ..
rm -rf kant-study # 현재 vscode 에서 실행 중이라서 열려있는 폴더는 삭제가 안됨. vscode 닫고 터미널에서 새로 입력해야 폴더까지 삭제됨

git clone https://github.com/metdori/kant-study.git # 주소에 업로드 해놓은 것을 현재 디렉터리에 다운로드함. Logs 폴더를 ignore 했던 상태라 업로드가 안 됐어서 사라졌음
cd kant-study
code . 
```

- `git add .` = 현재 폴더 기준 하위폴더 전체 추가
- `git add -A` 에서 `-A` = `--all` = 현재 프로젝트 전체 추가
- `@git 커밋 메시지 컨벤션` = 커밋할 때 어떻게 적을지 정해놓은 약속

```bash
git add .
git commit -m "docs: 깃 초기화 명령어 추가 (WIP)" # (Work In Progress 작업 진행 중이지만 일단 올린다는 의미)
git push origin main
```

***

## 5. Git Reset 및 Reflog 복구

```bash
git reset --soft 5839759 # 5839759 이 해쉬값으로 돌아감, --soft = working directory, staging area 둘 다 남아있음. 커밋만 삭제
git reflog # reflog = reference logs, 모든 행동 리스트를 나타내는 명령어
git reset --hard HEAD@{1} # HEAD@{1} 행동을 실행했던 때로 되돌아감
git reset --mixed 5839759 # 5839759 이 해쉬값으로 돌아감, --mixed = working directory 남아있음, staging area, 커밋 둘 다 삭제 / 아무것도 안 적으면 --mixed 가 기본값
git status # 빨간색 글씨로 staging 에 add 하기 전 단계로 돌아갔음
```

- `@Github 에서 commit 내역이랑 해쉬값 확인 가능`

***

## 6. 브랜치(Branch) 생성, 전환 및 병합

```bash
git branch feature/login # feature 하위폴더로 login
git branch # branch 목록 확인
git checkout feature/login
git log --oneline # HEAD 가 feature/login 으로 이동함
git branch --delete feature-login # --delete = 삭제 시도, -D = 강제 삭제 / 작업 중인 브랜치는 삭제 불가능
git branch --list # branch 목록 확인
git checkout -b feature/login # branch 생성 및 이동

git checkout main # feature 브랜치에서 작업했던 모든 커밋들이 삭제됨 (※ main 시점으로 화면이 전환됨)

git merge feature/login # feature/login 브랜치를 현재 있는 main 에 merge
git push origin main
git branch -D feature/login # feature/login 강제 삭제
```

***

## 7. 핵심 개념 및 단축키 메모

### VS Code 단축키
- `Alt + 방향키` : 해당 줄 라인 이동
- `Alt + Shift + 방향키` : 해당 줄 복사
- `Ctrl + X` : 해당 줄 삭제

### 핵심 개념 정리
- `@ Commit` = 세이브파일 만들기
- `@ Branch` = 복사본 만들기
- `@ 로직` = 오류 났을 때 수습하기가 훨씬 수월하기 때문에 복사본 만들어서 작업하고 병합하는 로직을 이용
- `@ 주의` = 동일한 줄 수정한 다음에 병합할 경우 충돌 발생
