# KANT DAY 4

## 1. VS Code 단축키 및 메뉴

- `Ctrl + K, F` = vscode 현재 열린 폴더 닫는 단축키
- `File > Open Recent` = 최근 열었던 폴더 목록

***

## 2. 브랜치 생성 및 3-Way Merge

```bash
git checkout -b feature/shop
git add .
git commit -m "docs: 브랜치 생성 명령어 추가"
git log --oneline # 신규 커밋을 아직 push 안해서 main 보다 더 앞서있음
```

```text
af5f1b0 (HEAD -> feature/shop) docs: 브랜치 생성 명령어 추가
6e0be2c (origin/main, origin/HEAD, main) docs: Git 로그 명령어 추가
```

```bash
touch CREDITS.md # 보통 프로젝트 만들 때 기여한 사람들 목록을 적음
git status # 빨간색 글씨 = Untracked files
git add . # 초록색 글씨 = Added = staging area 에 추가함
git commit -m "feat: CREDITS.md 파일 생성"
git log --oneline # 다른 브랜치가 안 보임
git log --oneline --all # 모든 브랜치 로그가 보임
git log --oneline --all --graph # 모든 브랜치 로그를 보기 쉽게 그래프로 나타냄. 'git graph' 확장프로그램 설치하면 더 보기 쉽게 바뀜

git merge feature/shop # Fast-Forward Merge 가 아니라 3-Way Merge = 양쪽 브랜치 모두 새로운 커밋 생겨서 VIM Editer 자동 실행됨. i = 현재 커서 앞 INSERT MODE, a = 현재 커서 뒤 INSERT MODE, Shift + a = 맨 뒤에서 INSERT MODE
```

- `@ESC 로 INSERT MODE 종료하고 :wq, w = write 저장, q = quit = 나가기, ! = 강제로 실행)`

```bash
git log --oneline --all --graph

git checkout feature/shop # main 에 있던 CREDIT.md 파일이 목록에서 사라짐
git merge main # feature 에서도 main 에 있던 커밋을 병합함
git log --oneline --all # main, feature 모두 같은 commit 에 위치됨
```

***

## 3. Pull / Merge 충돌(Conflict) 해결

### 로컬 브랜치 간 충돌 발생 및 해결
```bash
git add . # feature 에서 md 파일 내용 수정함
git commit -m "docs: 최종일정 추가"
git checkout main
git add . # feature 이랑 동일한 파일, 동일한 줄 내용 수정함
git commit -m "docs: 메인 개발자 교체"
git merge feature/shop # 동일 파일, 동일 줄 겹쳐서 충돌 발생 / 오류 개수만큼 CONFLICT 표시, Automatic merge faiiled
```

- `@Resolve in Merge Edit 버튼 = 직접 오류 확인하고 수정하는 탭 = 오류 많이 발생했을 때 사용)`
- `@Accept Current Change | Accept Incoming Change 버튼 = 버튼으로 자동 수정`
- `@직접 지워서 수정 = 오류 생긴 텍스트 직접 수정`

```bash
git status # 빨간색 글씨로 both modified 뜸
git add .
git commit -m "fix: 병합 커밋 충돌 해결"
```

***

## 4. 팀 협업 및 원격 충돌 시나리오

```bash
mkdir kant-team
cd kant-team
git init
git remote add origin https://github.com/metdori/kant-team.git
```

- `@Github > kant-team Repo > Settings > Collaborators > Add people 로 팀원 이메일 초대`

```bash
git clone https://github.com/koda0909/kant-team.git # 팀장의 Repo Clone = 프로젝트 파일 생성
git pull origin main # 팀장이 origin pull = 이미 프로젝트 파일이 있는 상태에서 새로운 커밋만 내려받음
git add .
git commit -m "docs: 팀 프로젝트 설정할 내용 추가"
git push origin main # 팀원이 수정한 내용 push함
git pull origin main # 팀원이 수정한 내용을 팀장이 pull함
```

- `@양쪽 다 수정한 상태에서 push 같이 해도 pull 과 동일하게 오류 발생`
- `@git 에서 먼저 생성한 걸 clone 할 경우 자동으로 .git 저장소로 지정됨`

***

## 5. Git 환경 설정 및 핵심 개념 정리

### Git 환경 설정
- `@git config --list` = git 설정을 확인하는 명령어
- `@git config --global pull.ff yes` = pull 의 fast-forward 설정

### 병합 전략 4가지 비교
- `@fast-forward merge` = main 브랜치에 새로운 커밋이 없을 때, new 브랜치에만 있을 때 merge = 새로운 커밋 생성 X 그냥 커밋만 이어서 붙여짐
- `@3-way merge` = new 브랜치에도 있고 main 에도 새로운 커밋이 있을 때 merge = 새로운 커밋 자동 생성, 충돌 생길 경우 수정 해야함
- `@rebase and merge` = new 브랜치를 main 기반으로 재정렬해서 merge = 깔끔하게 main 안으로 모든 커밋이 들어감
- `@squash and merge` = new 브랜치의 커밋들을 새로운 하나의 커밋으로 만들어서 main 브랜치에 merge = 완전 깔끔하게 main 안으로 새로운 1개 커밋만 들어감

### 협업 및 브랜치 병합 로직
- `@ branch 작업 중에 main 에 변경이 생기면 branch 에서 먼저 main 결과를 가져와서 merge 해주고 이후에 최신 main 이 적용된 branch 를 main 에 merge 하는 로직`
- `@ main 이 과거형이면 ff 가능, 미래형이면 무조건 branch 에서 먼저 최신화하고 main 에서 최신화 적용된 branch 를 merge`
