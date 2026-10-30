# KANT DAY 5

## 프로그래밍 언어 번역 방식
- **컴파일 방식:** 소스 코드 전체를 기계어로 미리 번역한 뒤 실행 (예: C, C++)
- **인터프리터 방식:** 코드를 한 줄씩 그때그때 기계어로 번역하며 실행 (예: Python)

***

## 1. 패키지 매니저 uv 기초

### 설치 및 프로젝트 생성
```bash
# uv 패키지 매니저 검색 및 설치
winget search uv
# (Name: uv, ID: astral-sh.uv 설치)

# 프로젝트 폴더 생성 및 초기화
mkdir first-project
cd first-project

# uv init: Git 자동 연결 및 기본 브랜치 생성
# --no-package: src/ 폴더를 만들지 않는 비패키지(간단한 앱/스크립트) 모드
uv init --no-package

# VS Code 열기
code .
```

### Git 첫 커밋
```bash
git add .
git commit -m "feat: 프로젝트 초기화"
```

### 주요 자동 생성 파일
- `.gitignore`: 파이썬 개발 시 불필요한 캐시, 가상환경 파일 자동 제외 목록 등록
- `pyproject.toml`: 프로젝트 이름, 설명, 요구 파이썬 버전 등이 정의된 프로젝트 설계도
- `uv.lock`: 정확한 패키지 버전과 의존성이 고정된 설치 영수증 명세서

### 프로젝트 첫 실행
```bash
# main.py 실행 (이 시점에 격리 구역인 .venv 가상환경과 uv.lock 이 자동 생성됨)
uv run main.py
```

***

## 2. 파이썬 버전 관리 및 패키지 제어

### 특정 파이썬 버전으로 프로젝트 시작
```bash
mkdir second-project
cd second-project

# Python 3.13 버전 명시 초기화
uv init --no-package --python 3.13

# 설정 확인
cat .python-version   # 출력: 3.13
cat pyproject.toml    # requires-python = ">=3.13" 확인
uv run main.py
```

### 파이썬 버전 다운그레이드 (3.12 고정)
```bash
# 3.12로 고정 시도 (pyproject.toml 요구 조건이 >=3.13이라 충돌 에러 발생)
uv python pin 3.12

# pyproject.toml 수정 (Vim 편집기 또는 code . 으로 열어 requires-python 수정)
vi pyproject.toml
# requires-python = ">=3.12" 로 수정 후 저장 (:wq)

# 다시 3.12 고정 실행 (정상 반영)
uv python pin 3.12
uv run main.py
```

### 패키지 추가, 확인, 삭제
```bash
# 라이브러리 추가 (pyproject.toml 및 uv.lock 에 자동 반영)
uv add fastapi

# 의존성 트리 확인
uv tree --depth 1   # 직접 설치한 라이브러리만 1단계로 표시
uv tree             # 하위 의존 라이브러리까지 전체 표시

# 라이브러리 삭제 (설계도/영수증 수정 및 .venv 에서 실제 제거)
uv remove fastapi

# 의존성 강제 동기화 (설계도와 가상환경 일치 보장)
uv sync
```

***

## 3. 가상환경 활성화 및 터미널 제어

```bash
# 가상환경 수동 활성화 (Bash 터미널 기준)
source .venv/Scripts/activate

# 현재 명령어로 물려 있는 파이썬 실행 경로 확인 (.venv 가리킴)
which python

# 가상환경 비활성화
deactivate

# 비활성화 후 경로 확인 (컴퓨터 기본 파이썬으로 복귀)
which python

# 터미널 명령어 이력 확인 및 재실행
history
!숫자   # 해당 번호의 명령어 재실행

# 파이썬 REPL 쉘 진입 및 종료
python
exit()
```

### 파이썬 파일 실행 비교
```bash
touch hello.py

# 1. 터미널이 바라보는 파이썬으로 실행 (가상환경 꺼져 있으면 본체 파이썬으로 실행됨)
python hello.py

# 2. uv를 통한 실행 (가상환경 활성화 여부와 무관하게 무조건 .venv 파이썬 보장)
uv run hello.py

# 3. .venv 파이썬 쉘 직접 진입
uv run python
```

***

## 4. VS Code 단축키 및 주피터 노트북 설정

### 필수 단축키
| 단축키 | 기능 설명 |
| :--- | :--- |
| `Ctrl + Shift + X` | 확장 프로그램(Extensions) 마켓플레이스 열기 |
| `Ctrl + Shift + E` | 파일 탐색기(Explorer) 창 열기 |
| `Ctrl + Shift + P` | VS Code 전체 명령 팔레트(Command Palette) 열기 |

### 주피터 노트북 환경 구축
1. VS Code 확장 마켓플레이스에서 **Python**, **Jupyter** 설치
2. 프로젝트에 주피터 연동 패키지 추가 및 커밋:
```bash
uv add ipykernel
git add .
git commit -m "feat: 주피터노트북 환경 설정"

# 노트북 파일 생성
touch chapter01.ipynb
```

### 주피터 커널(엔진) 연결 및 실행
1. `chapter01.ipynb` 열기
2. 우측 상단 **Select Kernel** 클릭
3. **Python Environments** 선택
4. 별표(⭐ Recommended)가 붙은 **`.venv` 가상환경 파이썬** 클릭

### 노트북 전용 단축키
- **`+ Code` 버튼:** 새 코드 셀 추가
- **`Shift + Enter`:** 현재 셀 실행 후 아래에 새 셀 생성 및 이동
- **`Ctrl + Enter`:** 현재 셀만 바로 실행 (셀 위치 유지)
- **명령 모드 (`Esc` 누른 상태):**
  - `M`: 현재 셀을 마크다운(Markdown) 셀로 변환
  - `Y`: 현재 셀을 파이썬 코드 셀로 변환
  - `A`: 현재 셀 위에 새 셀 삽입
  - `B`: 현재 셀 아래에 새 셀 삽입
  - `D D`: 현재 셀 삭제

```python
# 노트북에서 현재 동작 중인 파이썬 엔진 실제 경로 확인 코드
import sys
print(sys.executable)
```

***

## 5. 파이썬 기초 문법

### 변수 명명 규칙
- **숫자로 시작 불가:** `1_name` (X)
- **공백 포함 불가:** `user name` (X)
- **하이픈 사용 불가:** `user-age` (X)
- **파이썬 예약어 사용 불가:** `if`, `def`, `class`, `for` (X)

### 표기법 종류
- **스네이크 케이스 (Snake Case):** 파이썬 변수/함수 표준 컨벤션 (`user_name`, `value_list`)
- **카멜 케이스 (Camel Case):** 자바스크립트 등에서 주로 사용 (`userName`, `valueList`)
- **파스칼 케이스 (Pascal Case):** 파이썬 클래스명 컨벤션 (`UserName`, `ValueList`)

### 기본 자료형 예시
```python
# 정수형 (int)
age = 25

# 실수형 (float)
pi = 3.14

# 불리언형 (bool)
is_alive = True

# 문자열 (str)
user_name = "KwakTaeEun"

# 리스트/배열 (list)
grade_type = [1, 2, 3, 4]
```
