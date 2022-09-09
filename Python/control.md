# 제어문

* 파이썬은 기본적으로 위에서부터 아래로 차례대로 명령을 수행

* 특정 상황에 따라 **코드를 선택적으로 실행(분기/조건)하거나 반복적으로 실행하는 제어가 필요!**

## ✅ 조건문

> 조건문은 **참/거짓을 판단**할 수 있는 조건식과 함께 사용

* 기본 형식
  
  ① 조건이 **'참'인 경우**
  
  ② **이외의 경우 else** (<u>`else`는 **선택적**으로 활용</u>할 수 있음)

```python
if <조건>:
    <조건이 참이면 실행하는 코드>
else:
    <조건이 거짓이면 실행하는 코드>
```

* 예시

```python
num = int(input())

if (num % 2 == 0): # 2로 나눠서 나머지가 0이면 짝수
    print('짝수')
else:
    print('홀수')
```

### 1️⃣ **복수 조건문**

> 복수의 조건식을 활용할 경우 `elif`를 활용

* 조건식을 동시에 검사하는 것이 아니라 **위에서부터 순차적으로 비교**❗

```python
dust = int(input())

if dust > 150:
    print('매우 나쁨')
elif dust > 80:
    print('나쁨')
elif dust > 30:
    print('보통')
else:
    print('좋음')
```

### 2️⃣ **중첩 조건문**

> 조건문은 다른 조건문에 **중첩되어 사용 가능**

```python
dust = int(input())

if dust > 150:
    print('매우 나쁨')
    if dust > 300:
        print('실외 활동을 자제하세요')
elif dust > 80:
    print('나쁨')
elif dust > 30:
    print('보통')
else:
    print('좋음')
```

## ✅ 조건 표현식 = 삼항 연산자

* **표현 방법 - `<참인 경우 값> if 조건 else <거짓인 경우 값>`**

```python
num = int(input())
result = '홀수' if num % 2 else '짝수'
```

## ✅ 반복문

> **특정 조건을 만족할 때까지 같은 동작을 계속 반복**하고 싶을 때 사용

### 1️⃣ **While문**

* **<mark>조건식이 참인 경우 반복</mark>적으로 코드를 실행**
  
  * 코드 블록이 모두 실행되고, **다시 조건식을 검사하며 반복적으로 실행됨!**
  
  * 무한 루프를 하지 않도록 <mark>**<u>종료 조건이 반드시 필요!</u>**</mark>

```python
a = 0
while a < 5:
    print(a)
    a += 1 # 복합 연산자 = 연산과 할당을 합쳐 놓은 것을 의미
print('끝')
```

### 2️⃣ **for문**

> **시퀀스(`string`, `int`, `tuple`, `range`)를 포함한 순회 가능한 객체(iterable)의 요소를 모두 순회!**

* **iterable**
  
  * **순회할 수 있는 자료형 - `string`, `int`, `dict`, `tuple`, `range`, `set` 등**
  
  * **순회형 함수 - `range`, `enumerate`**

```python
for fruit in ['apple', 'mango', 'banana']:
    print(fruit)
print('끝!')
'''
apple
mango
banana
끝!
'''
```

* **문자열(string) 순회**

```python
chars = input()

for char in chars: # for idx in range(len(chars))
    print(char)        # print(chars[idx])
```

* **Dictionary 순회**
  
  * **`.keys()`** - key만 구성
  
  * **`.values()`** - value만 구성
  
  * **`.items()`** - (key, value)의 튜플로 구성

```python
grades = {
    'john': 80,
    'eric': 90
}

for student, grade in grades.items():
    print(student, grade) # 튜플로 구성
```

* **enumerate 순회**

> **인덱스와 객체를 쌍으로 담은 열거형 객체 반환**
> 
> (참고) 인덱스는 0부터 시작

```python
members = ['철수', '영희']

for idx, number in enuerate(members):
    print(idx, number)
```

* **List Comprehension**❗❗

> **표현식과 제어문을 통해 특정한 값을 가진 리스트를 간결하게 생성하는 방법**

```python
cubic_list = []
for number in range(1, 4):
    cubic_list.append(number ** 3)
print(cubic_list)

# list comprehension
cubic_list = [number ** 3 for number in range(1, 4)]
```

* **Dictionary Comprehension**❗

```python
cubic_list = {}
for number in range(1, 4):
    cubic_dict[number] = number ** 3
print(cubic_dict)

# dictionary comprehension
cubic_dict = {number: number ** 3 for number in range(1,4)}
print(cubic_dict)
```

## ✅ 반복문 제어

### 1️⃣ **break**

> **break문을 만나면 반복문은 종료**된다.

```python
for i in range(10):
    if i > 1:
        print('0과 1만 필요해!')
        break
    print(i)
```

### 2️⃣ continue

> **continue 이후의 코드 블록은 실행하지 않고, 다음 반복을 수행**한다.

```python
for i in range(6):
    if i % 2 == 0:
        continue
    print(i)
'''
1
3
5
'''
```

### 3️⃣ pass

> **아무것도 하지 않고 통과**
> 
> 자리를 채우는 용도로 사용하고 **반복문이 아니어도 사용 가능**하다.

```python
for i in range(4):
    if i == 2:
        pass
    print(i)
'''
0
1
2
3
'''
```

* 차이점 - continue의 경우, 위 코드에서 0, 1, 3 이 출력된다!

### 4️⃣ for ~ else

> **반복문을 끝까지 실행한 이후에 else문 실행**

```python
for char in 'apple':
    if char == 'b':
        print('b!')
        break
else:
    print('b가 없습니다.') # break문으로 중단 안되기 때문에 else문 실행
```

* **else문은 break로 중단되는 경우 실행 ❌**❗
