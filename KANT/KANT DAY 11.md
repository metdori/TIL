# KANT DAY 11


## 가위 바위 보 게임 (3판 2선승제)

```python
import random # import 라는 명령어를 써서 파이썬 내장 모듈인 random 을 가져온다

# 번호와 손 저장한 딕셔너리
HANDS = {1: "가위", 2: "바위", 3: "보"} # 불변하는 상수이므로 대문자로 지정
# 게임의 종료 조건
GOAL_WINS = 2 # 불변하는 상수이므로 대문자로 지정


# 게임 타이틀 출력
def show_title(): # show_title 함수 생성
    print("="*40)
    print("가위/바위/보 게임")
    print("="*40)

# 플레이어 이름 입력
def ask_name(): # ask_name 함수 생성
    name = input("플레이어 이름을 입력하세요. ").strip() # input = 내장함수, .strip() = 문자열 앞뒤 공백 제거 메서드
    if name == "":
        name = "게스트"
    return name # 함수 밖으로 name 값을 ask_name() 에 저장

# 플레이어 번호 입력
def ask_number(): # ask_number 함수 생성
    while True:
        print("1. 가위 / 2. 바위 / 3. 보")
        answer = input("번호를 선택하세요. : ").strip()

        if answer in ("1", "2", "3"):
            return int(answer) # 정수값으로 함수 밖으로 저장

        print("1, 2, 3 중에서 입력하세요.")

# 승패 판정
# 1 - 1 / 2 - 2 / 3 - 3 무승부 조건
# 1 - 3 / 2 - 1 / 3 - 2 플레이어 승리 조건
# 반환값 : 무승부 / 승리 / 패배
def check_result(user_number, computer_number): # check_result 함수에 user_number, computer_number 매개변수 사용
    # 무승부 조건
    if user_number == computer_number:
        return "무승부" # return("무승부") 는 안됨. return() 이라는 함수가 존재하지 않아서 오류 발생
    # 플레이어 승리 조건
    if user_number == 1 and computer_number == 3: return "승리" # 가위 / 보
    if user_number == 2 and computer_number == 1: return "승리" # 바위 / 가위
    if user_number == 3 and computer_number == 2: return "승리" # 보 / 바위
    return "패배" # elif, else 생략해도 return 때문에 정상 작동

# 한 판의 게임 결과 출력
def show_result(name, user_number, computer_number, result):
    print("\n[게임 결과]")
    print(f"{name}: {HANDS[user_number]} vs 컴퓨터: {HANDS[computer_number]}")
    print(f"=> {result}")

# 현재 스코어 표시
def show_score(name, my_wins, computer_wins):
    print(f"\n현재 스코어 {name} {my_wins} : {computer_wins} 컴퓨터\n") # ex) 게스트 1 : 0 컴퓨터

# 최종 결과 표시
def show_final_result(name, my_wins, computer_wins):
    print("-"*40)
    print(f"[최종 결과] {name} {my_wins} : {computer_wins} 컴퓨터")
    if my_wins > computer_wins:
        print(f"==> {name} 최종 승리!")
    else:
        print("==> 컴퓨터 승리!")

# 승패 판결 목록 표시
def show_records(records):
    print("\n[판별 승패 목록]")
    # "1판 : "가위 vs 바위 => 패배"
    # "2판 : "가위 vs 바위 => 패배"
    # "3판 : "가위 vs 바위 => 패배"
    for number, record in enumerate(records, start=1): # for in = 순서대로 실행, enumerate = 번호 매겨서 나열하는 내장함수 ex: (1, "가위 vs 보")
        print(f"{number}판 : {record}") # 규칙상 항상 앞이 번호이므로 number = 1, record = "가위 vs 보"

# 게임 재시작 여부
def ask_play_again():
    while True:
        answer = input("\n다시 도전 (yes/no) : ").strip().lower() # lower() = 소문자로 변환
        if answer == "yes" or answer == "y":
            return True
        if answer == "no" or answer == "n":
            return False
        
        print("yes 또는 no 입력해주세요.")

# 게임 루프 (3판 2선승제)
def play_game(name):
    my_wins = 0
    computer_wins = 0
    records = [] # 승패 기록할 리스트
    
    # 둘 다 2승이 안 됐을 때 다음 판을 진행
    while my_wins < GOAL_WINS and computer_wins < GOAL_WINS:
        user_number = ask_number()
        computer_number = random.randint(1, 3) # 1, 2, 3
        # 승패 판정
        result = check_result(user_number, computer_number)

        if result == "승리":
            my_wins += 1
        elif result == "패배":
            computer_wins += 1

        # 승패 결과 목록에 저장
        records.append(f"{HANDS[user_number]} vs {HANDS[computer_number]} => {result}")

        show_result(name, user_number, computer_number, result)
        show_score(name, my_wins, computer_wins)

    show_final_result(name, my_wins, computer_wins)
    show_records(records)
    # 플레이어가 승리하면 True
    return my_wins == GOAL_WINS # == 같아? 라고 물어보는 거라 최종 승리여부에 따라 T/F 반환

# 게임의 전체 루프
def main():
    show_title()
    name = ask_name()
    # 연승 기록을 위한 변수
    co_wins = 0


    while True:
        my_won = play_game(name) # 최종 승리여부에 따라 T/F 로 변환
        if my_won: # 최종 승리해서 my_won = True 일 때, 즉 if True:
            co_wins += 1 # 연승기록 누적
        else:
            co_wins = 0 # 연승기록 초기화
        if co_wins >= 1:
            print(f"\n현재 연승: {co_wins}연승")
        
        if not ask_play_again(): # no 했으면 if not False, 즉 if True 라서 break 가 실행됨.
            break

    print("게임을 종료합니다.")

# 게임 시작
main()
```

## 부모 클래스
```python
cd 폴더드래그 # 폴더 위치 바로 붙여넣어짐
uv init --no-package # uv 파이썬 패키지 관리자 초기화
uv sync # .venv 가상환경 만들고 설정 파일(pyproject.toml)을 읽어서 필요한 파이썬과 라이브러리를 .venv 폴더에 실제로 설치 및 동기화
source .venv/Scripts/activate # 이 프로젝트의 가상환경으로 들어가는 스위치

touch class_study.ipynb # class_study 라는 주피터노트북 생성
uv add ipykernel # uv 에 주피터노트북의 파이썬 코드를 현재 가상환경에서 실행할 수 있도록 연결해주는 ipykernel 패키지를 설치



# 부모 클래스 (=Base Class, Super Class, Parent Class)
class Animal:
    # 공통 기능
    def breathe(self):
        print("숨을 쉽니다.")
    def sound(self):
        print("소리를 냅니다.")

# 상속 (Inheritance)
class Dog(Animal):
    # 상속받은 클래스에서만 있는 고유 기능
    def bark(self):
        print("멍멍")

dog1 = Dog() # 인스턴스 생성
dog1.breathe() # 부모 클래스로부터 물려받은 메서드를 그대로 사용가능
dog1.bark() # 자식 클래스의 고유기능

# 메서드 오버라이딩 (method overriding)
# 부모 클래스에서 정의된 메서드를 수정 또는 추가하는 기능(재정의)
class Cat(Animal):
    def sound(self):
        print("야옹") # 위 클래스의 값이 아닌 야옹으로 재정의

nabi = Cat()
nabi.sound()



class Person: # Person 클래스 생성
    def __init__(self, name, age):
        self.name = name # 이름표에 name 값 적기
        self.age = age

class Student(Person): # Student 클래스 = Person 클래스를 상속
    def __init__(self, name, age, major):
        super().__init__(name, age) # super() 통해서 부모클래스의 생성자를 접근해서 name 속성을 설정, self.name self.age 생략
        self.major = major

std1 = Student("손흥민", 32, "스포츠댄스")
print(std1.name) # 부모 클래스로부터 물려받은 속성
print(std1.major) # 자식 고유의 속성


class Secret:
    def __init__(self):
        self.__password = "1234" # .__ = my_password.__password 값 숨기기

my_password = Secret()
print(my_password._Secret__password) # 숨겨놔서 가운데 _클래스명 적어줘야 출력됨

class Wallet:
    def __init__(self):
        self.__money = 1000 # 숨겨진 데이터

    def get_money(self): # Getter = 값을 안전하게 전달하는 용도
        return self.__money

    def set_money(self, amount): # Setter = 값을 검증하는 용도
        if amount >= 0:
            self.__money = amount
        else:
            print("잘못된 값입니다.")

wallet = Wallet()
wallet.set_money(2000)
print(wallet.get_money())
```






