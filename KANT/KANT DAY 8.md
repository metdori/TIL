# KANT DAY 8

## 함수 (Function)


- 반복되는 로직을 합쳐놓은 문법
- 재사용 높일 수 있는 코드 기법
- 단독사용가능 print(), len(), ...
- 함수명 컨벤션 : snake_case
- 매개변수 (Parameter)

```python
def 함수이름(매개변수, 매개변수, ...): #define
    실행할 로직 #1
    실행할 로직 #2
    실행할 로직 #3



def say_hello(name, age, point=1000): # default parameter 는 항상 맨 뒤에 있어야 함
    print(f"안녕하세요. 저는 {name} 입니다. 나이는 {age}입니다.")
    print(f"초기 지급 포인트 {point}가 지급되었습니다.")

say_hello("손흥민", 30, 2000) # 기본값 1000 이 무시되고 지정값 2000 이 출력됨
```

```python
# 가변 인수
# 대체로 *args = 전달 인수들 사용

def add_all(*numbers): # 매개변수 (Parameter), * = 입력값을 튜플로 포장
    print(f"입력된 튜플: {numbers}")
    total = sum(numbers)
    print(f"합계 결과: {total}")

add_all(1, 2, 3) # 인수(argument) 라고 함
add_all(1, 2, 3, 4, 5)
add_all()




# 딕셔너리 가변 인수
# 대체로 **kwargs = keyword arguments 사용
def user_info(**data): # ** = 입력값을 딕셔너리로 포장
    print(f"전달된 딕셔너리 구조: {data}")
    for key, value in data.items(): (하나씩 출력 = 1줄씩 적힘)
        print(f"{key}: {value}")

user_info(name="스파르타", level=5, active=True) # 함수 호출




def check_ord_number(number):
    if number > 0:
        return "양수"
    else:
        return "음수" # return = 결괏값 되돌리기

print(check_ord_number(10))
print(check_ord_number(-20))



# 여러개 결괏값 변환

def calc_value(a, b):
    plus = a + b
    minus = a - b
    return plus, minus # 튜플로 리턴

result1, result2 = calc_value(10, 5)
print(f"덧셈: {result1}, 뺄셈: {result2}")

result = calc_value(10, 5)
print(f"튜플: {result}")
type(result)



# 평균 계산해서 반환하는 함수
# 평균 = 총합 / 갯수
def get_average(*scores):
    if len(scores) == 0:
        return 0

    # if not scrors:
    #     return
    
    # 합계 계산
    total = sum(scores)
    # 평균 계산
    avg = total / len(scores)
    # 결괏값 반환
    return avg

result1 = get_average(80, 90, 100)
result2 = get_average(75, 80, 92, 100, 55)
result3 = get_average()

print(f"3과목 평균: {result1}")
print(f"5과목 평균: {result2}")



# 미니 계산기
# 사칙 연산기

def add(a, b):
    # 덧셈
    return a + b

def minus(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b == 0:
        return "0으로 나눌 수 없습니다."
    return a / b

a = input("첫번째 숫자: ")
b = input("두번째 숫자: ")

print(f"덧셈 연산 {a} + {b}: {add(int(a), int(b))}")
print(f"뺄셈 연산 {a} - {b}: {minus(int(a), int(b))}")
print(f"곱셈 연산 {a} * {b}: {multiply(int(a), int(b))}")
print(f"나눗셈 연산 {a} / {b}: {divide(int(a), int(b))}")



```
## 변수 스코프 (유효범위)

- 전역 변수 : 함수 밖에서 선언하는 변수, 어디서나 접근이 가능
- 지역 변수 : 함수 내에서 선언하는 함수, 함수 안에서만 접근이 가능

```python
def local():
    local_var = 1 # 지역 변수
    print(local_var)
    
local()
# print(local_var) # 오류 발생



message = "안녕하세요. Kant몰에 오신 것을 환경합니다." # 전역 변수

def greeting():
    message = "장난감 코너는 3층에 있습니다." # 지역 변수
    print(message) # 지역 변수가 우선된다.

greeting()

print(message) # 지역 변수



# 전역 변수
value = "전역 데이터"

# Function =====================
def outer_function():
    value = "지역 변수 1"

    def inner_function():
        value = "지역 변수 2"
        print(value)

    inner_function()
    print(value)
# Function =====================

outer_function()
print(value) #전역 변수


# global 키워드

counter = 0

def inc_counter():
    # counter = 0
    global counter # 전역 변수를 참조하겠다고 선언
    counter += 1

inc_counter()
inc_counter()

print(counter)
```
## Lambda Expression 람다식

```python
lambda 매개변수: 로직
```
```python
# 일반함수
def add_five(x):
    return x + 5

print(add_five(10))

# 람다식
result = lambda x: x + 5
print(result(20))


# map = 모든 요소를 반환
numbers = [1, 2, 3, 4, 5]

result = list(map(lambda x: x + 10, numbers))
print(f"계산 결과: {result}")

# 리스트 컴프리핸션 = 리스트 압축해서 한 번에
print([x+10 for x in numbers])

# filter = 조건에 맞는 요소만 반환
filter_result = list(filter(lambda x: x % 2 == 0, numbers))
print(f"짝수 결과: {filter_result}")

print([x for x in numbers if x % 2 == 0])


# 정렬
players = [
    ("손흥민", 95),
    ("김민재", 60),
    ("이강인", 80)
]

print(sorted(players, key=lambda p: p[1])) # 오름차순, key 값을 p[1] 위치 튜플(점수)로 고정
print(sorted(players, key=lambda p: p[1], reverse=True)) #내림차순
print(max(players, key=lambda p: p[1])) # 최고 점수 추출



numbers = [2, 3, 4, 5, 6, 7]

# Lambda 식으로 모든 요소의 거듭제곱 추출
sqr_numbers = list(map(lambda x: x ** 2, numbers))
print(f"거듭제곱 결과값: {sqr_numbers}")

# 결과값이 20 이상인 값만 추출
sqr_numbers_over_20 = list(filter(lambda x: x >= 20, sqr_numbers))
print(f"거듭제곱값이 20 이상: {sqr_numbers_over_20}")
```




