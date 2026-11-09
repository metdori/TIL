# KANT DAY 10

## 가위/바위/보 게임 (3판2선승제)
```python

rps_game # 폴더 생성
cd rps_game # ll = ls -l 단축키
uv init --no-package # uv를 활용하는 파이썬 생성 폴더로 지정, --no-package = src/ 폴더 없이 생성, 파일을 배포할 목적이 아닐 때 사용


# 가위 바위 보 게임 (3판 2선승제)

import random

# 번호와 손 저장한 딕셔너리
HANDS = {1: "가위", 2: "바위", 3: "보"}

# 게임 타이틀 출력
def show_title():
    print("="*40)
    print("가위/바위/보 게임")
    print("="*40)

# 플레이어 이름 입력
def ask_name():
    name = input("플레이어 이름을 입력하세요. ").strip() # strip() = 문자열 앞뒤 공백 제거
    if name == "":
        name = "게스트" # 이름 입력 안하면 게스트라고 지정
    return name # 함수 밖으로 name 값을 ask_name()으로 반환

# 게임의 전체 루프
def main():
    show_title()
    name = ask_name() # ask_name()을 name 으로 지정

    computer_number = random.randint(1, 3) # 1, 2, 3
    print(f"{name}님, 컴퓨터는 {HANDS[computer_number]}를 냈습니다.") # () = 함수를 실행할 때 사용, [] = 데이터 안에 특정값을 꺼낼 때 사용
    


# 게임 시작
main() # main() 실행, () 있어야 main() 안으로 들어가서 값을 바로 불러옴
```
