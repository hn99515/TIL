# 2차원 배열

> 1차원 list를 묶어놓은 list

* 2차원 이상의 다차원 list는 차원에 따라 index를 선언

* **2차원 list의 선언 - 세로 길이(행의 개수), 가로 길이(열의 개수)를 필요로 함**

* Python 에서는 데이터 초기화를 통해 변수 선언과 초기화가 가능

### 📍 `arr = [[0, 1, 2, 3], [4, 5, 6, 7]]` - 2행 4열의 2차원 list

![](Array_2_assets/2022-09-10-18-34-33-image.png)

```python
N = int(input())
arr = [list(map(int, input().split())) for _ in range(N)]

# 3
# 1 2 3
# 4 5 6
# 7 8 9
# [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

```python
'''
3
123
456
789
'''
N = int(input())
arr = [list(map(int, input())) for _ in range(N)]
```

## ✅ 배열 순회

> n X m 배열의 n * m 개의 모든 원소를 빠짐없이 조사하는 방법

* **행 우선 순회**

```python
# 2차원 배열의 모든 원소에 대해
for i in range(n):
    for j in range(m):
        Array[i][j]
```

* **열 우선 순회**

```python
for i in range(m):
    for j in range(n):
        Array[j][i]
```

* **지그재그 순회**

```python
for i in range(n):
    for j in range(m):        
        Array[i][j + (m-1-2*j) * (i%2)] # or 짝수/홀수행 나눠서 접근
```

* **델타를 이용한 2차 배열 탐색**❗❗
  
  * **2차원 배열의 한 좌표에서 4가지 방향(상하좌우)의 인접 배열 요소를 탐색**

```python
di = [0, 1, 0, -1] # 우, 하, 좌, 상
dj = [1, 0, -1, 0]
N = 3
M = 4
arr = [[1, 2, 3, 4,], [4, 5, 6, 7]]
for i in range(N):
    for j in range(M):
        for k in range(4):
            ni = i + di[k]
            nj = j + dj[k]
            if 0 <= ni < N and 0 <= nj < M:    # 유효한 인덱스에서만 탐색!
                print(ni, nj)
```

* **전치행렬 - 대각선을 기준으로 뒤집기**

```python
arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

for i in range(3):
    for j in range(3):
        if i < j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
```

# 부분집합

> 유한 개의 정수로 이루어진 집합이 있을 때, **이 집합의 부분집합 중에서 그 집합의 원소를 모두 더한 값이 0이 되는 경우가 있는지 확인**하는 문제

* Ex.
  
  * [-7, -3, -2, 5, 8]이라는 집합이 있을 때, [-3, -2, 5]는 이 집합의 부분집합이면 `-3 + (-2) + 5 = 0` 이므로 참!

* 완전 검색 기법으로 부분집합 합 문제를 풀기 위해서는 **우선 집합의 모든 부분집합을 생성한 후에 각 부분집합의 합을 계산**해야 함

## ✅ 부분집합의 수

* **집합의 원소가 n개일 때 공집합을 포함한 부분집합의 수는 2\*\*n 개**이다.

* 각 원소를 부분집합에 포함시키거나 포함시키지는 않는 2가지 경우를 모든 원소에 적용
  
  * `{1, 2, 3, 4} = 2 * 2 * 2 * 2 = 16가지`

* loop를 이용하여 부분집합을 생성하는 방법 - 원소가 증가함에 따로 for문 증가(비효율)

```python
bit = [0, 0, 0, 0]
for i in range(2):
    bit[0] = i
    for j in range(2):
        bit[1]  = j
        for k in range(2):
            bit[2] = k
            for l in range(2):
                bit[3] = l
                print(bit)
```

## ✅ 비트 연산자

> 같은 비트 단위로만 연산 가능

* `&` - 비트 단위로 AND 연산 = 둘 다 1 일 때만 1이 나오고 나머지는 0
  
  * ‼`i & (1<<j)` - **i의 j번째 비트가 1인지 아닌지를 검사**‼
  
  * i = 7 = 0111 / j = 0 일 때 0001 > 값 존재 / j = 1 일 때 0010 > 값 존재 ...
  
  * **j번째  비트가 0이면 결과는 무조건 0**
  
  * `1111 & 0010` = 0010

* `|` - 비트 단위로 OR 연산

* `<<` - 피연산자의 비트 열을 왼쪽으로 이동시킴 = `x 2`
  
  * `1 << n` - **2n 즉, 원소가 n개일 경우의 모든 부분집합의 수를 의미**

* `>>` - 피연산자의 비트 열을 오른쪽으로 이동시킴 = `/ 2`

```python
arr = [3, 6, 7, 1, 5, 4]
n = len(arr)            # 원소의 개수

for i in range(1<<n):   # 1<<n - 부분집합의 개수
    for j in range(n):  # 원소의 수만큼 비트를 비교
        if i & (i<<j):  # i의 j번 비트가 1인 경우
            print(arr[j], end=', ')
    print()
print()
```

# 검색

> 저장되어 있는 자료 중에서 원하는 항목을 찾는 작업

* 목적하는 탐색 키를 가진 항목을 찾는 것
  
  * 탐색 키 - 자료를 구별하여 인식할 수 있는 키

* **검색의 종류 - 순차 검색, 이진 검색, 해시**

* 일렬로 되어있는 자료를 순서대로 검색하는 방법
  
  * 가장 간단하고 직관적
  
  * 순차구조로 구현된 자료구조에서 원하는 항목을 찾을 때 유용
  
  * 알고리즘이 단순하여 구현이 쉽지만,  **검색 대상의 수가 많은 경우에는 수행시간이 급격히 증가하여 비효율적**임

## ✅ 정렬되어 있지 않은 경우

* 검색 과정
  
  * 첫 번째 원소부터 순서대로 검색 대상과 키 값이 같은 원소가 있는 비교
  
  * 키 값이 동일한 원소를 찾으면 그 원소의 인덱스를 반환
  
  * **마지막에 이를 때까지 못 찾으면 검색 실패**

* **찾고자 하는 원소의 순서에 따라 비교횟수가 결정❗ = O(n)**

```python
def sequentialSearch(a, n, key):
    i = 0
    while  i < n and a[i] != key:
        i += 1
    if i < n:
        return i
    else:
        return -1


def search(arr, key, N):
    for i in range(N):
        if arr[i] == key:
            return i
    return -1
```

## ✅ 정렬되어 있는 경우

* 검색 과정
  
  * 자료를 순차적으로 검색하면서 키 값을 비교하여, 원소의 키 값이 검색 대상의 키 값보다 크면 찾는 원소가 없다는 것이므로 **더 이상 검색하지 않고 검색 종료**

* **찾고자 하는 원소의 순서에 따라 비교횟수가 결정❗ = O(n)**

```python
def sequentialSearch2(a, n, key):
    i = 0
    while i < n and a[i] < key:
        i += 1
    if i < n and a[i] == key:
        return i
    else:
        return -1


def search2(arr, key, N):
    for i in range(N):
        if arr[i] == key:
            return i
        elif arr[i] > key:
            return -1
return -1
```

## ▶ 이진 검색  (Binary Search)

> **자료의 가운데에 있는 항목의 키 값과 비교**하여 다음 검색의 위치를 결정하고 검색을 진행하는 방법

* **검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 수행함**

* <mark>**이진  검색을 하기 위해서는 자료가 정렬된 상태여야 함**</mark>❗

* 검색 과정
  
  * 자료의 중앙에 있는 원소를 선택
  
  * 중앙 원소의 값과 찾고자 하는 목표 값을 비교
  
  * **목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서만 새로  검색 <=> 크다면 자료의 오른쪽 반에 대해서만 새로 검색**
  
  * 찾고자 하는 값을 검색할 때까지 위 과정을 반복

* 구현
  
  * 검색 범위의 시작점과 종료점을 이용하여 검색을 반복 수행
  
  * **자료에 삽입이나 삭제가 발생한 경우 배열의 상태를 항상 정렬 상태로 유지하는 작업이 필요**

```python
def binarySearch(a, N, key)
    start = 0
    end = N - 1
    while start <= end:
        middle = (start + end) // 2
        if a[middle] == key:
            return true
        elif a[middle] > key:
            end = middle - 1
        else:
            start = middle + 1
    return false
```

* **재귀 함수 이용 방법** - 가능하긴 하지만 비추천

```python
def binarySearch2(a, low, high, key):
    if low > high:
        return False
    else:
        middle = (low  + high) // 2
        if key == a[middle]:
            return True
        elif key < a[middle]:
            return binarySearch2(a, low, middle-1, key)
        elif key > a[middle]:
            return binarySearch2(a, middle+1, high, key)
```

### 📌 인덱스

> **테이블에 대한 동작 속도를 높여주는 자료 구조**

* 인덱스를 저장하는데 필요한 디스크 공간은 보통 테이블을 저장하는데 필요한 디스크 공간보다 작다.
  
  * **why**❓ 인덱스는 키-필드만 갖고 있고, 테이블의 다른 세부 항목들은 갖고 있지 않음

* 배열을 사용한 인덱스
  
  * 대량의 데이터를 매번 정렬하면 프로그램의 반응은 느릴 수 밖에 없다. 이러한 대량 데이터의 성능 저하 문제를 해결하기 위해 인덱스를 사용

* **원본 데이터에 데이터가 삽입될 경우, 상대적으로 크기가 작은 인덱스 배열을 정렬하기 때문에 속도가 빠르다.**
  
  ![](Array_2_assets/2022-09-11-00-10-14-image.png)

# 선택 정렬

> 주어진 자료들 중 **가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환**하는 방식

* 정렬 과정
  
  * **주어진 리스트 중에서 최소값을 찾는다.**
  
  * **그 값을 리스트의 맨 앞에 위치한 값과 교환**
  
  * 맨 처음 위치를 제외한 나머지 리스트를 대상으로 위 과정을 반복

```python
def SelectionSort(a, N):
    for i in range(N-1):        # 구간 시작
        midIdx = i
        for j in range(i+1, N):    # 실제 최소값 인덱스 찾기
            if a[minIdx] > a[j]:
                midIdx = j
        a[i], a[minIdx] = a[minIdx], a[i]    # 맨 앞자리와 교


arr2 = [7, 2, 5, 3, 4, 6]
N = len(arr2)

for i in range(N-1):
    minIdx = i                  # 구 간의 맨 앞을 최소값으로 가정
    for j in range(i+1, N):     # 실제 최솟값 인덱스 찾기
        if arr2[minIdx] > arr2[j]:
            minIdx = j
    arr2[minIdx], arr2[i] = arr2[i], arr2[minIdx]   # 최솟값을 구간 맨 앞으로 변경

print(arr2)
```

* 시간 복잡도 = **O(n\*\*2)**

## ✅ 선택 알고리즘

* 저장되어 있는 자료로부터 **K번째로 큰 혹은 작은 원소를 찾는 방법을 의미**
  
  * **최소값, 최대값 혹은 중간값을 찾는 알고리즘을 의미**

* 선택 과정
  
  * 정렬 알고리즘을 이용하여 자료 정렬하기
  
  * 원하는 순서에 있는 원소 가져오기

* **k번째로 작은 원소를 찾는 알고리즘 = O(kn)**

```python
def select(arr, k):
    for i in range(k):
        minIdx = i
        for j in range(i+1, len(arr)):
            if arr[j] < arr[minIdx]:
                minIdx = j
        arr[i], arr[minIdx] = arr[minIdx], arr[i]
    return arr[k-1]
```

| 알고리즘      | 평균 시간   | 최악 시간   | 기법     | 비고                     |
|:---------:|:-------:|:-------:|:------:|:---------------------- |
| bubble    | O(n**2) | O(n**2) | 비교와 교환 | 코딩이 가장 쉬움              |
| counting  | O(n+k)  | O(n+k)  | 비교환 방식 | n이 비교적 작을 때만 사용 가능     |
| selection | O(n**2) | O(n**2) | 비교와 교환 | 교환의 횟수가 버블, 삽입정렬보다 작다. |
