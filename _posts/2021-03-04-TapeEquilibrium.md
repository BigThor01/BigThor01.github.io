---
title: "[Codility] Lesson03 - Time Complexity(TapeEquilibrium)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---
오늘은 Codility 사이트 Lesson3. Time Complexity 에 있는 TapeEquilibrium 문제를 풀어보자.

## 문제

 - 목적 : "Minimize the value |(A[0] + ... + A[P-1]) - (A[P] + ... + A[N-1])|."

TapeEquilibrium 문제는 길이 N 의 array A 가 주어질 때, 어느 분기점에서 양쪽의 합의 차이가 가장 작은지를 구하는 문제야.

주어진 A (길이 N) 안에는 임의의 정수들이 들어가있어. 임의의 0 < P < N 인 P 가 주어지면 이것을 분기점으로 A[0]~A[P-1], A[P]~A[N-1] 두 개로 나눌 수 있지.

문제의 목적은 이 양쪽의 합의 차이가 가장 작을 때의 값을 return 하는 거야.

예를들어, A = [3,1,2,4,3] 라면 분기점 P 에 따라서 다음의 값을 갖고, 1 을 return 하면 돼.

 - P = 1 → 7
 - P = 2 → 5
 - P = 3 → 1
 - P = 4 → 7

## 코드

 - 입력 : A (array consisting of N integers)
 - 출력 : Integer (Minimum value of |(A[0]+...+A[P-1]) - (A[P]+...+A[N-1])|)


분기점 P 에 대해서 좌/우 합을 반복적으로 구하면 연산량이 O(N^2) 으로 비효율적이야.

분기점이 한칸씩 뒤로 갈 때, 바로 이전의 결과를 그대로 사용할 수 있다는 것에서 다음과 같이 코드를 짤 수 있어. 

```python
def solution(A):
    # -sum(A[0:]) 을 초기값으로 dif 에 넣음.
    dif = [-sum(A)]
    
    # 0 < p < N 에서 반복
    for p in range(1, len(A)):
        # sum(A[:p])-sum(A[p:]) = sum(A:(p-1)) + A[p-1] - sum(A[(p-1):]) + A[p-1] = (sum(A:(p-1)) - sum(A[(p-1):])) + 2 * A[p-1] 
        dif.append(dif[p-1] + 2*A[p-1])
    
    # 구해진 dif 에서 0 < p < N 에 해당되는 dif[1:] 의 절대값을 취한 후, min 을 return
    return min(map(abs, dif[1:]))
```

코드 설명은 다음과 같아.

 1. 리스트 dif 에 -sum(A) 를 넣습니다.
 2. P = 1 인 경우, dif[0] + 2 * A[0] 을 구해서 dif 에 append 합니다.
 3. P = 2 인 경우, dif[1] + 2 * A[1] 을 구해서 dif 에 append 합니다.
 4. P = N-1 까지 반복합니다. 마지막은 dif[N-2] + 2 * A[N-1].
 5. dif 에서 0 번째를 제외하고 절대값으로 map 하고 그 중 min 을 구합니다. 


 - 코드 time complexity : O(N)


## 더 느린 코드

```python
def solution(A):
    dif = []
    for i in range(1, len(A)-1):
        dif.append(abs(sum(A[:i]) - sum(A[i:])))
    
    return min(dif)
```

 - 코드 time complexity : O(N^2)

