
# KANT DAY 6

## Python 특징

변수 재할당 Reassignment (변수 이름은 그대로 둔 채, 가리키는 값을 새 객체로 바꾸는 행위)  

Garbage Collector (Python 은 C언어와 다르게 Garbage 를 자동으로 삭제해줌)  

```python
money = 5000
money = money + 2000
print(money)
```

입력하면 7000, 9000 ... 주피터 노트북에선 코드 실행마다 값이 갱신됨  

주피터노트북 위에 Clear All Outputs = 모든 결과 삭제  
주피터노트북 위에 Run All = 위부터 전부 실행  

***

Github cli 사이트에서 설치주소 복사  

```bash
winget install --id GitHub.cli # (github cli 설치, --id = 고유 id값과 일치하는 프로그램 설치)

gh repo list # (auth login 하라고 오류뜸)
gh auth login # (one-time login code 를 웹사이트에 입력하면 연결됨)

gh repo list # (github 에 업로드되어있는 repo 목록 나옴)

gh repo create kant_python --private # (kant_python 레포를 private 로 생성)

git remote add origin https://github.com/metdori/kant_python

gh repo delete https://github.com/metdori/kant_python # (권한 얻으려고 gh auth refresh -h github.com -s delete_repo 입력하라고 뜸)

gh repo delete https://github.com/metdori/kant_python # (권한 얻고 다시 실행하면 삭제됨)
```

Ctrl + Shift + P -> create: new jupyter notebook 눌러서 새로 생성  
Ctrl + S -> chapter02.ipynb 이름으로 저장  

Alt + 방향키 = 해당 줄 이동  
Alt + Shift = 방향키 = 해당 줄 복사  

***

주피터노트북 왼쪽 아래 Outline 으로 MD 형식으로 바로 이동 가능  
% = 나머지, // = 몫, ** 거듭제곱, += 복합대입 연산자, == = 양쪽이 같은지 참거짓, != = 같지 않다, ord = Ordinal = 아스키 및 유니코드 번호  

%s = 문자열 자리수, %d = 정수 자리수, %5d = 정수, 총 5칸, 남은 건 공백처리, %.4f = 실수, 소숫점 4자리까지, :>10, = > 오른쪽 정렬, 총 10칸, 1000단위 쉼표  

```python
format_str = "주문내역: {0} {1} -> 총 {2}원".format(name, qty, price) # (format 방식)
format_str = f"주문내역: {name} {qty} -> 총 {(price * qty):>10,}원" # (f-sting 방식)
```

***

### 리스트 (수정 가능)

- 대괄호 [ ]

### 튜플 (수정 불가능 = 데이터 안정성, 괄호 생략 가능, 리스트보다 파일 크기 작음)

- 소괄호 ( )

### 딕셔너리 (수정 가능, 고유한 키 값)
 
- 중괄호 { }

```python
# 리스트 : 가변 데이터 저장
shopping_list = ["apple", "banana", "cherry"]

print(shopping_list) # 리스트 출력
print(shopping_list[0]) # 리스트 첫번째 요소 출력

shopping_list[0] = "kiwi" # 리스트 첫번째 요소 변경
print(shopping_list[0]) # 리스트 첫번째 요소 출력
print(shopping_list) # 바뀐 요소 리스트 출력
```

### 리스트 매서드

```python
# append
fruits = ["apple"]
fruits.append("banana") # 리스트 마지막에 요소 추가
print(fruits)

# insert
fruits.insert(0, "kiwi") # 리스트 첫번째 위치에 요소 삽입
print(fruits)

# remove
# fruits.remove("apple") # 리스트에서 apple 요소 제거
# print(fruits)

# fruits.remove("grape") # 리스트에 없는 요소 제거 시 에러 발생
```

***


### 튜플 예시

```python
axis = (38.123, 41.123) # 튜플이므로 요소 고정

# axis[0] = 36.1234 # 튜플이므로 요소 변경하면 에러 발생


point = 1, 2, 3 # 튜플은 괄호 생략 가능
print(point, type(point)) # 튜플 요소랑 타입 확인 = (1, 2, 3) <class 'tuple'>


import sys # 파이썬 내장 모듈 sys 불러오기
print(sys.getsizeof([1, 2, 3])) # 해당 리스트에 대한 메모리 사용량 확인 = 88
print(sys.getsizeof((1, 2, 3))) # 해당 튜플에 대한 메모리 사용량 확인 = 72
```

리스트보다 튜플의 사용량이 더 작음  

***

### 인덱스

```python
colors = ["red", "green", "blue"]


- 정방향 : 0, 1, 2 # 맨 처음은 0
- 역방향 : -3, -2, -1 # 맨 마지막은 -1


colors = ["red", "green", "blue"]
print(colors[0]) # 리스트 첫번째 요소 출력
print(colors[-1]) # 리스트 마지막 요소 출력
```

***

### 슬라이싱

```python
numbers = [0, 1, 2, 3, 4, 5]

# 1번부터 4번미만까지 추출 = 1, 2, 3 추출
numbers[1:4]
# 맨 처음부터 2칸 간격으로 추출 = 0, 2, 4 추출
numbers[::2]
# 역순으로 추출 = 5, 4, 3, 2, 1, 0 추출
numbers[::-1]
# 앞에서 3개만 추출 = 0, 1, 2 추출
numbers[:3]
# 3번부터 끝까지 추출 = 3, 4, 5 추출
numbers[3:]

print ((10, 20, 30, 40)[1:3]) # 튜플에 슬라이싱 적용 가능


matrix = [
    [1, 2, 3],
    [4, 5, 6]
]

print (matrix[1]) # 2차원 리스트의 1번째 요소 출력
print (matrix[1][2]) # 2차원 리스트의 1번째 요소의 3번째 요소 출력
print (len(matrix), len(matrix[0])) # 2차원 리스트의 행과 첫번째 열의 요소 개수 출력


user_roles = ["admin", "member", "guest"]

print (len(user_roles)) # user_roles 리스트의 요소 개수 출력
print ("admin" in user_roles) # user_roles 리스트에 admin 요소가 있는지 확인
```

***

Ctrl + / = 해당 줄 # 주석처리 단축키
