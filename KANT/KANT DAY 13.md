# KANT DAY 13

## `if __name__ == "__main__":`

1. `__name__`의 정체
- 파이썬이 스크립트를 실행할 때 자동으로 부여하는 "내장 특별 변수(이름표)"
- 어떤 방식으로 파일이 열렸느냐에 따라 들어가는 값이 달라진다. 직접 실행한 파일 = 무조건 __main__ / 불려간 파일 = 해당 파일명

2. 사용하는 이유
- "단독 실행용 코드"와 "외부 임포트용 부품 코드"를 분리할 수 있다
- `if __name__ == "__main__":` 아래에 작성한 코드는 내가 터미널에서 직접 실행할 때만 동작하고, 내 파일을 `import`로 쓸 때는 자동으로 숨겨져 실행되지 않는다
---
- 파이썬 내장 모듈을 지정하고 F12 누르면 코드 내부로 들어가진다 or Ctrl + Click
---
## from ~ import ~
```python
cd config.py

# 초기 상태 값
START_HUNGER    = 30
START_HAPPY     = 70
START_ENERGY    = 70
```
```python
cd pet.py

import random
from config import START_ENERGY, START_HAPPY, START_HUNGER # config.py 에서 정보를 가져옴

class Pet:
    # 클래스 변수
    count = 0 # 지금까지 입양한 펫 수를 기록

    def __init__(self, name): # __init__ = 객체 초기 세팅 생성자
        self.name = name # 입력한 이름을 self.name 으로 지정
        self.hunger = START_HUNGER # 배고픔 (0 ~ 100)
        self.happy = START_HAPPY # 행복도 (0 ~ 100)
        self.energy = START_ENERGY # 에너지 (0 ~ 100)
        self.alive = True
        Pet.count += 1
```
## import ~
```python
import random
import config # import 항목이 적을 경우 이런 방식도 사용 가능, 변수 앞에 모듈명 붙여줘야 함 ex. config.START_HUNGER

class Pet:
    # 클래스 변수
    count = 0 # 지금까지 입양한 펫 수를 기록

    def __init__(self, name): # __init__ = 객체 초기 세팅 생성자
        self.name = name # 입력한 이름을 self.name 으로 지정
        self.hunger = config.START_HUNGER # 배고픔 (0 ~ 100)
        self.happy = config.START_HAPPY # 행복도 (0 ~ 100)
        self.energy = config.START_ENERGY # 에너지 (0 ~ 100)
        self.alive = True
        Pet.count += 1
```

## 쇼핑몰 장바구니




## DOCKER 리눅스 세팅
```powershell
wsl --install # Windows Subsystem for Linux, 대부분의 docker 컨테이너가 리눅스 기반으로 작동하므로 설치 필요
winget search docker
winget install Docker.DockerDesktop
```
