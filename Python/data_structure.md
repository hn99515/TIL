# 데이터 구조 (Data Structure)

## ✅ 데이터 구조 활용

> **데이터 구조를 활용하기 위해서는 메서드(method)를 활용**
> 
> 메서드는 클래스 내부에 정의한 함수 = 사실상 함수와 동일

* **사용법**
  
  * **`데이터 구조.메서드()`**

### 1️⃣ 순서가 있는 데이터 구조

#### ① 문자열 - <u>변경 불가능 (immutable)</u>

#### 📌 문자열 조회/탐색 메서드 & 검증 메서드

| 문법               | 설명                                                |
| ---------------- |:------------------------------------------------- |
| **`s.find(x)`**  | **x의 첫 번째 위치를 반환.** <u>없으면 -1을 반환</u>             |
| **`s.index(x)`** | **x의 첫 번째 위치를 반환.** <u>없으면 오류 발생</u>              |
| `s.isalpha()`    | 알파벳 문자 여부 <br><u>*유니코드상 Letter면 확인 가능(한국어 포함)</u> |
| `s.isupper()`    | 대문자 여부                                            |
| `s.islower()`    | 소문자 여부                                            |
| `s.istitle()`    | 타이틀 형식 여부 = 첫 번째 대문자 + 나머지 소문자                    |

```python
print('apple'.find('p')) # 1
print('apple'.find('k')) # -1
print('apple'.index('p')) # 1
print('apple'.index('k')) # ValueError
```

```python
print('abc'.isalpha()) # True
print('ㄱㄴㄷ'.isalpha()) # True
print('Ab'.isupper()) # False
print('ab'.islower()) # True
print('Title Title!'.istitle()) # True
```

#### 📌 문자열 변경 메서드(s는 문자열을 의미)

| 문법                                   | 설명                                                              |
| ------------------------------------ | --------------------------------------------------------------- |
| **`s.replace(old, new[, count])`**   | **바꿀 대상 글자를 새로운 글자로 바꿔서 반환!**<br><u>count를 지정하면 해당 개수만큼만 시행</u> |
| **`s.strip([chars])`**               | **공백이나 특정 문자를 제거**                                              |
| **`s.split(sep=None, maxsplit=-1)`** | **공백이나 특정 문자를 기준으로 분리**                                         |
| **`'separator'.join([iterable])`**   | **구분자로 iterable 을 합침**                                          |
| `s.capitalize()`                     | 가장 첫 번째 글자를 대문자로 변경                                             |
| `s.title()`                          | 문자열 내 띄어쓰기 기준으로 각 단어의 첫글자는 대문자로, 나머지는 소문자로 변환                   |
| `s.upper()`                          | 모두 대문자로 변경                                                      |
| `s.lower()`                          | 모두 소문자로 변경                                                      |
| `s.swapcase()`                       | 대<->소문자 서로 변경                                                   |

```python
print('woooowooo').replace('o', '!', 2)) # w!!oowooo
print('    와우!'.strip()) # '와우!'
print('안녕하세요????'.rstrip('?')) # '안녕하세요'
print('a b c'.split()) # ['a', 'b', 'c']

msg = 'hI! Everyone, I\'m ssafy'

print(msg.capitalize()) # Hi! everyone, i'm ssafy
print(msg.title()) # Hi, Everyone, I'M Ssafy
print(msg.swapcase()) # Hi! eVERYONE, i'M SSAFY
```

#### 📌 문자열은 immutable인데... 문자열 변경이 되는 이유는❓

* **기존의 문자열을 변경하는 것이 아니라, <u>변경된 문자열을 새롭게 만들어서 반환!</u>**

```python
word = 'python'
print(word) # python
print(id(word)) # 2006262763184 (주소 값을 가리키고 있다가)
print(word.upper()) # PYTHON
print(id(word.upper()) # 2006262763120 (다른 주소 값을 가리킨다.)
```

#### ② 리스트

> **여러 개의 값을 순서가 있는 구조로 저장하고 싶을 때 사용**

| 문법                           | 설명                                                                                |
| ---------------------------- | --------------------------------------------------------------------------------- |
| **`L.append(x)`**            | **리스트 마지막에 항목 x를 추가**                                                             |
| **`L.insert(i, x)`**         | **리스트 인덱스 i에 항목 x를 삽입**<br>**인덱스 i가 리스트 길이보다 큰 경우 맨 뒤에 삽입**                       |
| `L.remove(x)`                | **리스트 가장 왼쪽에 있는 항목 x를 제거.** <u>항목이 존재하지 않는 경우 ValueError</u>                      |
| **`L.pop()`**                | **리스트 내 마지막을 반환 후 제거**                                                            |
| `L.pop(i)`                   | 리스트의 인덱스 i에 있는 항목을 반환 후 제거                                                        |
| **`L.extend(iterable)`**     | **순회형 m의 모든 항목들이 리스트 끝에 추가**<u>(+=과 같음)</u><br>**문자열로 넣게 되면 문자 하나씩 쪼개서 리스트에 추가됨** |
| **`L.index(x, start, end)`** | 리스트에 있는 항목 중 가장 왼쪽에 있는 항목 x의 인덱스를 반환                                              |
| `L.reverse()`                | 리스트를 거꾸로 정렬 <u>(크기 역순으로 정렬한다는 의미 ❌)</u>                                           |
| **`L.sort()`**               | **리스트를 정렬**<u>(매개변수 이용가능)</u>                                                     |
| **`L.count(x)`**             | **리스트에서 항목 x가 몇 개 존재하는지 갯수를 반환**                                                  |

```python
cafe = ['starbucks', 'tomntoms', 'hollys']
cafe.append('twosome')
print(cafe) # ['starbucks', 'tomntoms', 'hollys', 'twosome']

cafe.insert(0, 'start')
print(cafe) # ['start', 'starbucks', 'tomntoms', 'hollys', 'twosome']

cafe.extend(['coffee']) # 'coffee' 로 넣으면 문자열이 쪼개져서 들어간다!
print(cafe) # ['start', 'starbucks', 'tomntoms', 'hollys', 'twosome', coffee']

numbers = ['hi', 1, 2, 3]
numbers.remove('hi') 
print(numbers) # [1, 2, 3]
numbers.remove('hi') # ValueError

numbers.pop()
print(numbers) # [1, 2]

print(numbers.index(100)) # ValueError

number = [1, 2, 3, 1, 1, 1, 2, 2]
print(number.count(1)) # 4
print(number.count(100)) # 0
```

#### 📍 <u>**sort vs sorted**</u>❗

```python
numbers = [3, 2, 5, 1]
result = numbers.sort() # sort는 원본을 변경!
print(numbers, result) # [1, 2, 3, 5] None
re = sorted(numbers) # sorted는 원본 변경 없고, 새로운 리스트 반환
print(numbers, re) # [3, 2, 5, 1] [1, 2, 3, 5]
```

#### ③ **튜플**

> **여러 개의 값을 순서가 있는 구조로 저장하고 싶을 때 사용**

#### 📌리스트와의 차이점?

* **<u>생성 후 담고 있는 값 변경이 불가! (immutable)</u>**

* 변경할 수 없기 때문에 **값에 영향을 미치지 않는 메서드만을 지원**

```python
day_name = ('월', '화', '수', '목', '금')
print(type(day_name)) # tuple
print(day_name[-3]) # 수
print(day_name * 2) # 연산 가능

day_name += False, True
print(day_name) # ('월', '화', '수', '목', '금', False, True)
```

* 멤버십 연산자 - 포함 여부를 확인 **(`in`, `not in`)**

```python
print('apple' in 'a') # False
print('a' in 'apple') # True
print('hi' in 'hi I am ssafy') # True
print('서순' in ['서순', '순서', '기러기']) # True
```

### 2️⃣ 비시퀀스형 데이터 구조

#### ① set

> 중복되는 요소가 없이 순서에 상관없는 데이터의 묶음 (mutable)

* **순서가 없기 때문에 인덱스를 이용한 접근 불가능**

* **집합 연산이 가능!**

| 문법                  | 설명                                                                        |
| ------------------- | ------------------------------------------------------------------------- |
| **`s.copy()`**      | **set의 얕은 복사본을 반환**                                                       |
| **`s.add(x)`**      | **항목 x가 set에 없다면 추가**                                                     |
| **`s.pop()`**       | **랜덤하게 항목을 반환하고, 해당 항목을 제거.** <u>set이 비어있을 경우 KeyError</u>                |
| `s.remove(x)`       | **항목 x를 set에서 삭제.** <u>항목이 존재하지 않을 경우 KeyError</u>                        |
| **`s.discard(x)`**  | **항목 x가 set에 있는 경우, 항목 x를 set에서 삭제.**  <u>항목이 없는 경우 삭제해도 에러가 발생하지 않음!</u> |
| `s.update(t)`       | **set t에 있는 모든 항목 중 set s 에 없는 항목을 추가**<br>여러 값을 추가                       |
| `s.clear()`         | 모든 항목을 제거                                                                 |
| `s.isdisjoint(t)`   | set s가 set t의 서로 같은 항목을 하나라도 갖지 않는다면 True 반환                              |
| **`s.issubset(t)`** | set s가 set t의 하위 셋인 경우, True 반환                                           |
| `s.issuperset(t)`   | set s가 set t의 상위 셋인 경우, False 반환                                          |

```python
a = {'사과', '바나나', '수박'}
a.add('딸기')
print(a) # {'바나나', '딸기', '사과', '수박'}

a.update(['딸기', '바나나', '참외'])
print(a) # {'참외', '바나나', '딸기', '수박', '사과'}

a.remove('애플')
print(a) # KeyError
a.discard('애플') # 없는 값을 삭제해도 에러가 발생하지 않음!
print(a) # {'참외', '바나나', '딸기', '수박', '사과'}
a.pop()
print(a) # 임의의 원소 하나를 제거해 반환
```

```python
a = {'사과', '바나나', '수박'}
b = {'포도', '망고'}
c = {'사과', '포도', '망고', '수박', '바나나'}

print(a.isdisjoint(b)) # True, a와 b는 서로소인가?
print(b.issubset(c)) # True, b가 c의 하위 집합인가?
print(a.issuperset(c)) # False, a가 c의 상위 집합인가?
```

#### ② 딕셔너리

> **key는 변경 불가능한 데이터(immutable)만 활용 가능**

* **immutable data = string, integer, float, boolean, tuple, range**

* value - 어떠한 형태든 관계 없음

| 문법                      | 설명                                                                            |
| ----------------------- | ----------------------------------------------------------------------------- |
| `d.clear()`             | 모든 항목을 제거                                                                     |
| `d.copy()`              | **dict d의 얕은 복사본을 반환**                                                        |
| **`d.keys()`**          | **dict d의 모든 키를 담은 뷰를 반환**                                                    |
| **`d.values()`**        | **dict d의 모든 값을 담은 뷰를 반환**                                                    |
| **`d.items()`**         | **dict d의 모든 키-값의 쌍을 담은 뷰를 반환**                                               |
| **`d.get(k)`**          | **키 k의 값을 반환.** <u>k가 없는 경우 None 반환</u>                                       |
| **`d.get(k, v)`**       | **키 k의 값을 반환.** <u>k가 없는 경우 v를 반환</u>                                         |
| **`d.pop(k)`**          | **키 k의 값을 반환하고 k인 항목을 dict d에서 삭제**하는데, <u>키 k가 dict d에 없는 경우 KeyError 발생</u> |
| `d.pop(k, v)`           | 키 k의 값을 반환하고 k인 항목을 dict d에서 삭제하는데, <u>키 k가 dict d에 없는 경우 v를 반환</u>           |
| **`d.update([other])`** | **dict d의 값을 매핑하여 업데이트**                                                      |

```python
my_dict = {'apple' : '사과', 'banana' : '바나나'}
my_dict['pineapple'] # KeyError

print(my_dict.get('pineapple')) # None
print(my_dict.get('pineapple', 0)) # 0

data = my_dict.pop('apple') # key가 없을 때 default 값이 없으면 KeyError
print(data, my_dict) # 사과 {'banana': '바나나'}

my_dict = {'apple': '사', 'banana': '바나나'}
my_dict.update(apple='사과') # 함수의 매개변수는 문자열 형태로 작성 X
print(my_dict) # {'apple': '사과', 'banana': '바나나'}
```

## ✅ 얕은 복사와 깊은 복사 (Shallow & Deep)

### 1️⃣ 할당

* 대입 연산자 `=` : **해당 객체에 대한 객체 참조를 복사❗ = 얕은 복사❗**

```python
original_list = [1, 2, 3]
copy_list = original_list
print(original_list, copy_list) # [1, 2, 3] [1, 2, 3] = 주소 공유

copy_list[0] = 'hello'
print(original_list, copy_list) # ['hello', 2, 3] ['hello', 2, 3]
```

### 2️⃣ 얕은 복사(shallow copy)

* **slice 연산자 활용**하여 같은 원소를 가진 리스트지만 연산된 결과를 복사 (다른 주소)

```python
# 얕은 복사를 해결할 수 있는 방법 - slice
a = [1, 2, 3]
b = a[:] # 새로운 리스트를 생성하는 법 1
# b = list(a) - 새로운 리스트를 생성하는 법 2

print(a, b) # [1, 2, 3] [1, 2, 3]

b[0] = 'a'
print(a, b) # [1, 2, 3] ['a', 2, 3]
```

* 복사하는 리스트의 원소가 주소를 참조하는 경우

```python
a = [1, 2, ['a', 'b']] # 2차원 리스트일 때 얕은 복사의 문제가 또 발생
b = a[:] # 2차원 리스트인 경우 슬라이싱을 하더라도 문제 발생
print(a, b) # [1, 2, ['a', 'b']] [1, 2, ['a', 'b']]

b[2][0] = 0
print(a, b) # [1, 2, [0, 'b']] [1, 2, [0, 'b']]
```

### 3️⃣ 깊은 복사(deep copy)

```python
import copy

a = [1, 2, ['a', 'b']]
b = copy.deepcopy(a) # deepcopy를 통해 해결 가능
print(a, b) # [1, 2, ['a', 'b']] [1, 2, ['a', 'b']]

b[2][0] = 0
print(a, b) # [1, 2, ['a', 'b']] [1, 2, [0, 'b']]
```
