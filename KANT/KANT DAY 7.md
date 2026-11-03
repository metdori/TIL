# KANT DAY 7

## 딕셔너리 (Dictionary)

- {"키":"값"} 형식, : 없으면 집합 
- "키" 중복 불가
- "값" 중복 가능
- 추가하는 순서를 유지

```python
user_info = {
    "name": "KwakTaeEun",
    "age": 23
}

print(user_info["age"]) # 키로 값을 출력
user_info["age"] = 30 # 값을 수정함. 키는 유지
print(user_info["age"]) # 수정된 값이 출력
user_info["role"] = "Tutor" # 키를 추가함
print(user_info) # 키를 추가한 값이 추가돼서 출력됨
```

### 삭제

```python
user_info = {
    "name": "KwakTaeEun",
    "age": 23,
    "role": "admin"
}

print(user_info)

del user_info["role"] # 키를 삭제함

print(user_info) # 키값이 삭제됨


age = user_info.pop("age") # pop : 딕셔너리 밖으로 꺼내면서 삭제하는 메서드
print (age, user_info)
```

### 딕셔너리 주요 메서드

- Keys(), values(), items() # 각 목록을 조회하는 메서드


```python
menu = {"coffee" : 4000, "tea": 4500}

print(menu.keys()) # key 목록 조회
print(menu.values()) # value 목록 조회
print(menu.items()) # items 목록 조회 (키, 값) 튜플 형태 묶음으로 리턴
```

### 딕셔너리는 인덱서 방식으로 값을 참조 불가 > list 로 변환
```python
print (list(menu.keys())[1]) # 해당 키들 리스트 중 2번쨰 자리 출력

print (sum(menu.values())) # 해당 키의 값이 합쳐진 값 출력

print ("coffee" in menu) # in 은 T/F 형식 출력 = True
print ( 4000 in menu) # 값은 조회 불가 = False
print ( 4000 in menu.values()) # 딕셔너리 목록 조회한 건 인식 가능 = True

menu["cake"] # 없는 키이므로 오류 발생

price = menu.get("cake", 5000) # get: 해당 키의 값을 가져오는 메서드, 대체값 지정 가능
print(price) # cake 값이 없지만 대체값 5000 지정했으므로 5000 출력

print(menu.get("cake")) # 기본값이 없는 경우 None 출력
print(menu.get("coffee", 0)) # 해당 키 값이 있으면 그 값을 출력, 없을 경우만 대체값 0 출력
```

### 집합 (Set)

수학 집합 : 중복 없고, 순서 없는 자료형
- 중괄호 사용 (: 있으면 딕셔너리, 없으면 집합)
- set() 함수를 생성

```python
grade = set([1, 2, 2, 3, 3, 3])
print (grade) # 중복된 건 합쳐서 나옴 = {1, 2, 3}

print (type({})) # {} 빈 딕셔너리 타입 출력 = <class 'dict'>
print (type(set())) # set() 빈 집합 타입 출력 = <class 'set'>
print ({1, 2, 2}) # 집합 출력하면 중복 자동 제거 = {1, 2}

s = {1, 2}

s.add(2)
s.add(5) = 집합 추가
print(s) = {1, 2, 5}

s.remove(1) # remove = 없는 키 지우면 오류 발생
print(s) = {2, 5}

s.discard(2) # discard = 없는 키 지워도 오류 발생 X
print(s) = {5}


set_a = {1, 2, 3}
set_b = {3, 4, 5}

print(set_a & set_b) # & = 교집합
print(set_a | set_b) # | = 합집합
print(set_a - set_b) # - = 차집합 = {1, 2}
print(3 in set_a) # 포함 여부 T/F = True
```

### 중복제거

```python
raw_emails = ["a@sparta.com", "b@sparta.com", "a@sparta.com"] # 원시 데이터

unique_set = set(raw_emails) # 집합으로 만들어서 중복 제거

print(list(unique_set)) = 집합 리스트 출력하므로 중복 삭제됨

print("# 중복제거 및 순서유지")
sort_emails = list(dict.fromkeys(raw_emails)) # dict.fromkeys() = 중복제거 및 입력 순서유지
print(sort_emails)

print(sorted(set(["c", "d", "a", "f", "a"]))) # sorted = 중복제거 및 가나다순 정렬
```


## 제어문 (반드시 들여쓰기 필요)

```python
if 조건식:
    실행할 코드 #1
    실행할 코드 #2
    실행할 코드 #3
else:
    실행할 코드 #4

if 조건식1:
    실행할 코드 #1
    실행할 코드 #2
    실행할 코드 #3
elif 조건식2:
    실행할 코드 #4
    실행할 코드 #5
else:
    실행할 코드 #6
```

### 다양한 조건식

```python
cart = ["카메라", "삼각대"]

if cart:
  print("결제를 진행합니다.")
   print("결제를 진행합니다. #2") # 들여쓰기 길이 다르면 오류 발생
else:
    print("장바구니가 비었습니다.")


if True:
    pass # pass = 그냥 패스함
```
### 한줄 표기법
```python
age = 19
message = "성인" if age >= 20 else "미성년자" 
message
```
### if 조건문 순서
```python
score = 95

if score >=  80: # if 조건문 만족하므로 밑에 elif 실행 X
    print("B")
elif score >= 90:
    print("A")

if score >= 90:
    print("A")
elif score >= 80:
    print("B")



is_login = True
has_permission = True

if is_login and has_permission:
    print("관리자 페이지 이동")

x = 10
if x == 10: # = 는 대입 연산자, == 는 비교 연산자
    print(f"x는 {x} 입니다.") # f = 포맷 문자열, {x} 에 변수 x 값이 들어감
```
### for 반복문 (데이터를 하나씩 수행)

```python
for 변수 in 변수할_데이터:
    반복수행할 코드 #1
    반복수행할 코드 #2


fruits = ["사과", "바나나", "체리"]

for fruit in fruits:
    print(fruit)
```
사과
바나나
체리

```python
menu = {"아아": 4000, "홍차": 4500}

menu.keys()
for key in menu:
    print(key)

for name, price in menu.items():
    print(f"{name} : {price}")
```

### range() = 숫자 0부터 -1번까지 반환

```
for i in range(5): = i 값에 0~4 하나씩 입력
    print(f"{i+1} 번째 로직 수행")


for i in range(1, 6, 2): # 범위 지정 1부터 5까지 2칸씩 뛰어서 반환
    print(i) = 1, 3, 5 1줄씩 출력

print(range(5)) = range(0, 5) 출력
print(list(range(5_000))) # range 범위를 리스트로 변환 후 출력, 읽기 편하게 _ 가능
```
### while 문 (실패할 경우 없으면 무한 실행)

```python
while 조건식:
    반복할 로직 #1
    반복할 로직 #2
    반복할 로직 #3


num = 1

while num < 3:
    print(f"현재 숫자는 {num}")
    num += 1


count = 3
while count > 0:
    print(count)
    count -= 1 # count = count - 1



for num in range(1, 6):
    if num == 3:
        print("즉시 종료")
        break
    print(f"현재 num={num}")



while True: # 무한 실행
    user_input = input() # 키보드 입력값을 변수에 넣음
    if user_input == "q":
        print("종료")
        break



for num in range(1, 6):
    if num % 2 == 0: # 짝수 (2 로 나눴을 때 나머지가 0)
        continue
    print(f"홀수 {num}")



# 구구단

for x in range(2, 10):
    if x == 4:
        break

    print(f"--- {x}단 ---")
    for y in range(1, 10):
        if y == 6:
            break
        print(f"{x} x {y} = {x * y}")



# 회원 데이터

user_database = [
    {"name" : "손흥민", "level": "VIP", "active": True, "point": 15000}, 
    {"name" : "김민재", "level": "Gold", "active": False, "point": 9000}, 
    {"name" : "조규성", "level": "VIP", "active": True, "point": 12000}, 
    {"name" : "이강인", "level": "Gold", "active": True, "point": 8000}, 
    {"name" : "호명보", "level": "Bronze", "active": False, "point": 1000}
]

# VIP 포인트 누적 변수
total_vip_points = 0
# 현재 활성 회원 수
active_user_count = 0

print("=== 마케팅 대상 활성 VIP 및 Gold 회원 결과")

for user in user_database:
     # 활성화 된 고객 필터링 active
     if user["active"]:
        # 활성 회원수 누적
        active_user_count += 1

        if user["level"] == "VIP":
            total_vip_points += user["point"]

            print(f"[VIP 등급] {user['name']} 회원님에게 VIP 쿠폰이 발행되었습니다.")
        elif user["level"] == "Gold":
            print(f"[Gold 등급] {user['name']} 회원님에게 Gold 쿠폰이 발행되었습니다.")

print(f"전체 활성 회원 수 : {active_user_count}명")
print(f"활성 VIP 포인트 누적 : {total_vip_points}점")




import random # 난수 발생을 위한 모듈 임포트

# 1 ~ 100 사이의 난수 발생
number = random.randint(1, 100) # randint = random + int = 무작위 정수 출력

# 시도 횟수
try_count = 0

# 타이틀
print("="*45)
print("컴퓨터가 1부터 100사이의 숫자를 생성했습니다")
print("="*45)

# 메인 루프
while True:
    user_input = input("컴퓨터의 숫자를 맞춰보세요 (1 ~ 100): ")
    if user_input == "q":
        print(f"게임 종료합니다. 정답은 {number}입니다.")
        break

    if not user_input.isdecimal():
        print("숫자만 입력해주세요.")
        continue

    user_number = int(user_input)
    # 시도 회수 증가
    try_count += 1 # try_count++, ++try_count 은 python 에서 X

    if user_number < 1 or user_number > 100:
        print("1부터 100사이값을 입력하세요.")
        continue

    # 입력값과 정답을 비교
    if user_number < number:
        print("up! 더 큰 수입니다.")
    elif user_number > number:
        print("Down! 더 작은 수입니다.")
    else:
        print(f"정답입니다. {try_count}회 만에 성공했습니다.")
        break # 정답일 경우 무한 루프 탈출
```
