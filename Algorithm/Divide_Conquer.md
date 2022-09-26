# 분할 정복(Divide & Conquer)

> 문제 제시: 가짜 동전 찾기

* n 개의 동전들 중에 가짜 동전이 하나 포함되어 있다. 가짜 동전은 진짜 동전에 비해 조금 가볍다. 양팔 저울을 최소로 사용해서 가짜 동전을 찾는 방법은?

* 예) 동전이 24개(진짜 23개, 가짜 1개)가 있다면?
  
  * 12개씩 반으로 나눠서 저울 활용하면 가짜동전 있는 위치 확인 가능!

## ▶ 전략

* 분할 - 해결할 문제를 여러 개의 작은 부분으로 나눈다.

* 정복 - 나눈 작은 문제를 각각 해결한다.

* 통합 - 해결된 해답을 모은다.

* Top-down 접근

![](Divide_Conquer_assets/2022-09-26-12-33-37-image.png)

## ▶ 거듭 제곱

* 반복 알고리즘: O(n)

![](Divide_Conquer_assets/2022-09-26-13-39-01-image.png)

* **분할 정복 기반의 알고리즘: O(log N)**

![](Divide_Conquer_assets/2022-09-26-13-39-42-image.png)

## ▶ 병합 정렬(Merge Sort)

> 여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식

* 분할 정복 알고리즘 활용
  
  * 자료를 최소 단위의 문제까지 나눈 후에 차례대로 정렬하여 최종 결과를 얻어냄
  
  * top-down 방식

* **시간 복잡도 = O(n log n)**

* **병합 정렬 과정**
  
  * {69, 10, 30, 2, 16, 8, 31, 22}를 병합 정렬
  
  * **분할 단계** - 전체 자료 집합에 대하여, **최소 크기의 부분집합이 될 때까지 분할 작업을 계속 진행함**
    
    ![](Divide_Conquer_assets/2022-09-26-12-38-33-image.png)
  
  * **병합 단계** - **2개의 부분집합을 정렬하면서 하나의 집합으로 병합**
  
  * 8개의 부분집합이 1개로 병합될 때까지 반복함
    
    ![](Divide_Conquer_assets/2022-09-26-12-40-06-image.png)

* 분할 과정

```python
def sort(arr):
    # 리스트 길이가 1이면 최소 단위이므로 종료
    if len(arr) == 1:
        return arr
	
    # 중간 위치 찾기
    mid = len(arr) // 2
    
    # 중간을 기준으로 왼쪽, 오른쪽 분할
    l_list = arr[:mid]
    r_list = arr[mid:]
	
    # 분할한 리스트를 정렬
    l_list = sort(l_list)
    r_list = sort(r_list)
	
    # 정렬된 두 리스트를 병합
    return merge(l_list, r_list)
```

* 병합 과정

```python
def merge(l, h):
	# 분할한 리스트를 병합하기 위한 리스트
    res = []
	
    # 분할한 두 리스트의 대소비교를 하며 result 리스트에 담는 것을 반복
    while len(l) > 0 or len(h) > 0:
        
        # 왼쪽, 오른쪽 리스트 모두 비교할 대상이 남아있을 경우
        if len(l) > 0 and len(h) > 0:
            if l[0] <= h[0]:
                res.append(l[0])
                l = l[1:]
            else:
                res.append(h[0])
                h = h[1:]
		
        # 오른쪽 리스트는 끝, 왼쪽 리스트에만 숫자가 남았을 경우
        elif len(l) > 0:
            res.append(l[0])
            l = l[1:]

        # 왼쪽 리스트는 끝, 오른쪽 리스트에만 숫자가 남았을 경우
        elif len(h) > 0:
            res.append(h[0])
            h = h[1:]
	
    # 합친 결과를 반환
    return res

```

```python
def merge_sort(arr):
    def sort(low, high):
    if high - low < 2:
        return
    mid = (low + high) // 2
    sort(low, mid)
    sort(mid, high)
    merge(low, mid, high)


    def merge(low, mid, high):
    temp = []
    l, h = low, mid
    
    while l < mid and h < high:
        if arr[l] < arr[h]:
            temp.append(arr[l])
            l += 1
        else:
            temp.append(arr[h])
            h += 1

    while l < mid:
        temp.append(arr[l])
        l += 1
    while h < high:
        temp.append(arr[h])
        h += 1

    for i in range(low, high):
        arr[i] = temp[i-low]

return sort(0, len(arr))
```



# 퀵 정렬

> 주어진 배열을 두 개로 분할하고, 각각을 정렬한다.

## 📌 병합 정렬과 다른점

* 병합 정렬은 그냥 두 부분으로 나누는 반면에, **퀵 정렬은 분할할 때 기준 pivot 중심으로, 그보다 작은 것은 왼편, 큰 것은 오른편에 위치**시킨다.

* 각 부분 정렬이 끝난 후, **병합정렬은 '병합'이라는 후처리 작업이 필요하나, 퀵 정렬은 필요하지 않다.**

* 알고리즘

```python
def quickSort(arr, l, r):
    if l < r:
        s = partition(arr, l, r)    # pivot
        quickSort(arr, l, s-1)
        quickSort(arr, s+1, r)
```

```python
# Hoare-Partition 알고리즘
def partition(arr, pivot, high):
    i = pivot + 1
    j = high

    while True:
        while i < high and arr[i] > arr[pivot]
            i += 1
        while j > pivot and a[j] > arr[pivot]
            j -= 1
        if j <= i:
            break
        arr [i], arr[j] = arr[j], arr[i]

        i += 1
        j -= 1
    
    arr[pivot], arr[j] = arr[j], arr[pivot]
    return j
```

## ▶ 아이디어

* P(피봇) 값들 보다 큰 값은 오른쪽, 작은 값들은 왼쪽 집합에 위치

![](Divide_Conquer_assets/2022-09-26-13-00-51-image.png)

* 피봇을 두 집합의 가운데에 위치

![](Divide_Conquer_assets/2022-09-26-13-01-03-image.png)

* 피봇 선택 - 왼쪽 끝/오른쪽 끝/임의의 세 개 값 중에 중간 값 = 원하는 대로 설정 가능

![](Divide_Conquer_assets/2022-09-26-13-02-15-image.png)

![](Divide_Conquer_assets/2022-09-26-13-02-40-image.png)

![](Divide_Conquer_assets/2022-09-26-13-03-07-image.png)

![](Divide_Conquer_assets/2022-09-26-13-03-42-image.png)

* pivot 자리 정한 후 좌측과 우측 구간에 대해 quickSort 재진행!

![](Divide_Conquer_assets/2022-09-26-14-07-21-image.png)

* `Lomuto Partition` 알고리즘

```python
def partition(arr, pivot, r):
    x = arr[r]
    i = pivot - 1
    for j in range(pivot, r-1):
        if arr[j] <= x:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i+1], arr[r] = arr[r], arr[i+1]
    return i+1
```

![](/Users/awake-ukey/Library/Application%20Support/marktext/images/2022-09-26-21-45-01-image.png)

* quicksort 기본형

```python
def partition(l, r):
    pivot = A[l]
    i, j = l, r
    while i <= j:
        while i <= j and A[i] <= pivot:
            i += 1
        while i <= j and A[j] >= pivot:
            j -= 1
        if i < j:
            A[i], A[j] = A[j], A[i]
    A[l], A[j] = A[j], A[l]
    return j


def qsort(l, r):
    if l < r:
        s = partition(l, r)
        qsort(l, s-1)
        qsort(s+1, r)


A = [7, 2, 5, 3, 4, 5]
N = len(A)
qsort(0, N-1)
print(A)
```

# 이진 검색 (Binary Search)

> **자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법**

* 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 수행

* **이진 검색을 하기 위해서는 자료가 정렬된 상태여야 한다.**

* 검색 과정
  
  * 1️⃣ 자료의 중앙에 있는 원소를 고른다.
  
  * 2️⃣ 중앙 원소의 값과 찾고자 하는 목표 값을 비교
  
  * 3️⃣ **목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행하고, 크다면 자료의 오른쪽 반에 대해서 새로 검색을 수행**
  
  * 4️⃣ 찾고자 하는 값을 찾을 때까지 1️⃣~3️⃣ 의 과정을 반복

![](Divide_Conquer_assets/2022-09-26-14-39-33-image.png)

![](Divide_Conquer_assets/2022-09-26-14-39-52-image.png)

* 반복구조

```python
def binarySearch(arr, key):
    start = 0
    end = len(arr) - 1

    while start <= end:
        mid = int((start + end) // 2)
        if arr[mid] == key: # 검색 성공
            return mid
        elif arr[middle] > key:
            end = mid - 1
        else:
            start = mid + 1
    return -1

arr = [2, 4, 7, 9, 11, 19, 23]

print(binarySearch(arr, 7))   # 2
print(binarySearch(arr, 20))  # 검색 실패
```

* 재귀구조

```python
def binarySearch(arr, start, end, key)
    if start > high:
        return -1
    else:
        mid = (low + high) // 2
        if key == arr[mid]:
            return mid
        elif key < arr[mid]:
            return binarySearch(arr, start, mid - 1, key)
        else:
            return binarySearch(arr, mid + 1, end, key)
```

## 📌 분할 정복의 활용

* 병합 정렬은 외부 정렬의 기본이 되는 정렬 알고리즘
  
  * 멀티코어 CPU나 다수의 프로세스에서 정렬 알고리즘을 병렬화하기 위해 병합 정렬 알고리즘을 활용

* 퀵 정렬은 매우 큰 입력 데이터에 대해서 좋은 성능을 보이는 알고리즘

# 백트래킹 (Back-tracking)

> N-Queen 문제 = NxN 체스판에서 Queen들이 서로 위협하지 않도록 n개의 Queen을 배치하는 문제

## ▶ 개념

* 당첨 리프 노드 찾기
  
  * 루트에서 갈 수 있는 노드를 선택
  
  * 꽝 노드까지 도달하면 최근의 선택으로 되돌아와서 다시 시작
  
  * 더 이상의 선택지가 없다면 이전의 선택지로 돌아가서 다른 선택
  
  * 루트까지 돌아갔을 경우 더 이상 선택지가 없다면 찾는 답이 없다.

![](Divide_Conquer_assets/2022-09-26-14-56-09-image.png)

### 📌 백트래킹과 깊이 우선 탐색(DFS)와의 차이

* 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 더 이상 그 경로를 따라가지 않음으로써 시도의 횟수를 줄임 = **Prunning (가지치기)**

* **깊이 우선 탐색이 모든 경로를 추적 / 백트래킹은 불필요한 경로를 조기에 차단**

* 깊이 우선 탐색을 가하기에는 경우의 수가 너무나 많음. **즉 N! 가지의 경우의 수를 가진 문제에 대해 깊이 우선 탐색을 가하면 처리 불가능함**

* **백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만 이 역시 최악의 경우에는 지수함수 시간 복잡도이므로 처리 불가능**

## ▶ 8-Queens 문제

* 후보 해의 수 = `64C8` = 4,426,165,368 개

* 실제 해 = 92개

* **4-Queens 문제로 축소해서 생각하기!**
  
  * **같은 행에 위치할 수 없다.**
  
  * **모든 경우의 수 = 4x4x4x4 = 256**

![](Divide_Conquer_assets/2022-09-26-15-04-22-제목%20없음.png)

* **백트래킹 기법**
  
  * 어떤 노드의 유망성을 점검한 후에 유망하지 않으면 그 노드의 부모로 되돌아가 다음 자식 노드로 감
  
  * **가지치기(Prunning) - 유망하지 않는 노드가 포함되는 경로는 더 이상 고려하지 않음**

## ▶ 알고리즘

```python

```

* 상태 공간 트리

![](Divide_Conquer_assets/2022-09-26-15-09-21-image.png)

## ▶ {1, 2, 3}의 부분집합을 구하는 백트래킹 알고리즘

```python

```

![](Divide_Conquer_assets/2022-09-26-15-16-31-image.png)

* {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}의 부분집합 중 원소의 합이 10인 부분집합을 모두 출력

```python
N = 3
arr = [1, 2, 3]
check = [0] * N

def powerset(idx):
    if idx == N:
        print(check, ":", end = ' ')
        for i in range(N):
            if check[i]:
                print(arr[i], end=' ')
        print()
        
        return
    
    # idx 자리의 원소를 뽑는 경우
    check[idx] = 1
    powerset(idx + 1)
    # idx 자리를 안뽑는 경우
    check[idx] = 0
    powerset(idx + 1)


powerset(0)
```

## ▶ 백트래킹을 이용한 순열 구하기

> 접근 방법은 앞의 부분집합 구하는 방법과 유사

```python
arr = [1, 2, 3]
n = 3
sel = [0] * n
check = [0] * n

# 재귀방식
def perm(idx):
    
    # 다 뽑아서 정리했다면
    if idx == n:
        print(sel)
    
    else:
        for i in range(n):
            if check[i] == 0:
                sel[idx] = arr[i]   # 값을 사용
                check[i] = 1        # 사용함 표시
                perm(idx+1)
                check[i] = 0        # 다음 반복문을 위한 원상복구

perm(0)
# =>
# [1, 2, 3]
# [1, 3, 2]
# [2, 1, 3]
# [2, 3, 1]
# [3, 1, 2]
# [3, 2, 1]

# --------- #

# 비트연산 방식
# check 10진수 정수
def perm(idx, check):
    if idx == n:
        print(sel)
        return

    for i in range(n):
        # i 번째 원소를 활용했군, 그럼 안쓰고 넘어가지
        if check & (1<<i): continue

        sel[idx] = arr[i]
        perm(idx+1, check | (1<<i)) # 원상복구가 필요없다...

perm(0,0)
# => 결과는 위와 동

# ---------------- #

# 스왑 방식
def perm(idx):
    if idx == n:
        print(arr)

    else:
        for i in range(idx, n):
            # 순서를 바꾸고
            arr[idx], arr[i] = arr[i], arr[idx]
            perm(idx + 1)
            
            # 원상복구
            arr[idx], arr[i] = arr[i], arr[idx]

perm(0)

```

![](Divide_Conquer_assets/2022-09-26-15-19-05-image.png)
