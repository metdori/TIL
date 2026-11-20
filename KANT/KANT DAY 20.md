# KANT DAY 20

## NumPy Pandas

### NumPy(Numerical Python)
- 파이썬의 기본 리스트(list)보다 훨씬 빠르고 효율적으로 수학 연산을 처리하기 위한 라이브러리
- 반복문(for) 없이 배열 전체에 한 번에 사칙연산 적용 가능
---
### Pandas
- 엑셀 시트나 SQL 테이블처럼 행(row)과 열(column)이 있는 표 형태 데이터를 쉽고 직관적으로 다룰 수 있게 해주는 라이브러리
- 조건에 맞는 행 필터링, 정렬, 그룹화(groupby), 표 병합(merge / join)
---

### Numpy Pandas 연습
- Numpy Pandsa 설치
```python
uv add numpy pandas ipykernel # ipykernel = 노트북의 python 실행 커널
```
- numpy, pandas 모듈 임포트, 버전 확인
```python
import numpy as np
import pandas as pd

print("numpy:", np.__version__)
print("pandas:", pd.__version__)
```
- 리스트에서 amounts * 2 = 결괏값이 곱해지지 않고 아니라 요소 2번 반복
```python
amounts = [3200, 1500, 15000, 6500, 32000, 4800,
            10900, 1500, 2700, 3000, 18500, 7000]

print(amounts)
print("합계", sum(amounts))

print(amounts * 2)
print("==="*3)
```
- NumPy 배열에서 arr * 2 = 리스트랑 다르게 결괏값이 곱해짐
```python
import numpy as np
arr = np.array(amounts) # np.array = NumPy의 array 기능
print(arr * 2) # np.array 로 인해 리스트와 다르게 결괏값이 곱해짐

print("합계:", arr.sum())
print("평균:", arr.mean())
print("최소:", arr.min())
print("최대:", arr.max())

print("모양:", arr.shape, "데이터 타입:", arr.dtype) # shape = 배열 모양을 튜플로 표시, 배열의 데이터 타입
```
- 모양: (12,) 데이터 타입: int64 / 숫자 12개가 있는 1차원, 64비트짜리 정수 / (1, 5) = 1행, 5열의 2차원
---
- 일반 리스트: [1000, 2000] * 2 -> [1000, 2000, 1000, 2000]
- NumPy 배열: np.array([1000, 2000]) * 2 -> [2000, 4000]
---
```python
print(arr >= 10000) # 조건 만족하는 값을 True, False 로 출력
print(arr[arr >= 10000]) # True 인 값들을 출력
```
```python
amounts = [3200, 1500, 15000, 6500, 32000, 4800,
            10900, 1500, 2700, 3000, 18500, 7000]

shops = ["편의점", "버스", "영화관", "분식집", "온라인쇼핑", "카페",
         "음원앱", "버스", "편의점", "웹툰", "온라인쇼핑", "분식집"]

categories = ["식비", "교통", "문화", "식비", "쇼핑", "식비",
              "문화", "교통", "식비", "문화", "쇼핑", "식비"]

print(shops)

food = 0
for c, a in zip(categories, amounts): # zip = 여러 리스트에서 같은 위치의 값끼리 짝지어 묶어주는 파이썬 내장 함수
    if c == "식비":
        food = food + a # 식비인 값을 계속 더함

print(food)
```
```python
import pandas as pd

spend = pd.DataFrame({ # DataFrame = numpy 의 array 처럼 pandas 의 2차원 표를 만드는 대표적인 도구
    "day": [2, 3, 4, 5, 7, 9, 12, 14, 16, 19, 20, 25],
    "shop": shops,
    "category": categories,
    "amount": amounts
})
spend.head(5)
spend.info()
# print(spend.shape) # (12, 4) 12행, 4열의 표
```
- 파이썬 괄호 역할 = () 함수 실행(호출), 튜플 / {} 딕셔너리, 집합 / [] 데이터 꺼내기(인덱싱), 리스트
```python
spend["amount"]
print("합계:", spend["amount"].sum()) # amount 값 합침
print("평균:", round(spend["amount"].mean(),2)) # amount 값 mean 평균내고 round ,2 소수점 2번째까지 표시
```
```python
# spend["shop", "amount"] # [] 안에 있는 값을 모두 인식 즉 "shop, amount" 로 인식해서 오류남
# spend[ 컬럼명 ]
# spend[ [컬럼1, 컬럼2, ...] ] # 이런 식으로 적어야 "shop" "amount" 따로 인식
spend[ ["shop", "amount"] ]
```
- 조건 설정
```python
spend["category"] == "문화" # 해당하는 값을 True, False 로 출력
```
```python
spend[spend["category"] == "문화"] # 해당하는 값을 T / F 출력하고 T 값의 row 만 출력
```
```python
spend[ (spend["category"] == "식비") & (spend["amount"] >= 5000) ] # 파이썬에서 & 이 계산 최우선 순위이므로 ()로 나눠줘야 함
```
```python
top3 = spend.sort_values("amount", ascending=False).head(3) # 기본적으로 ASC 이므로 ASC=False 로 DESC 로 설정
```
```python
spend["point"] = spend["amount"] // 100 # 100원마다 1 포인트 // 나누기 몫, % 나누기 나머지
spend.head(5)
print(spend["point"].sum())
```
```python
spend.loc[2, "amount"] = 14000
spend["point"] = spend["amount"] // 100
spend.head(3)
```
```python
spend.groupby("category")["amount"].sum()
```
```python
spend.groupby("category")["amount"].sum().sort_values(ascending=False)
```
```python
spend["category"].value_counts()
```
