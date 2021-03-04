---
title: "[Codility] Lesson03 - Time Complexity(TapeEquilibrium)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---


## 코드

```python
def solution(A):
    # -sum(A[0:]) 을 초기값으로 dif 에 넣음.
    dif = [-sum(A)]
    
    # 0 < p < N 에서 반복
    for p in range(1, len(A)):
        # sum(A[:p]) - sum(A[p:]) = sum(A:(p-1)) + A[p-1] - sum(A[(p-1):]) + A[p-1] = (sum(A:(p-1)) - sum(A[(p-1):])) + 2 * A[p-1] 
        dif.append(dif[p-1] + 2*A[p-1])
    
    # 구해진 dif 에서 0 < p < N 에 해당되는 dif[1:] 의 절대값을 취한 후, min 을 return
    return min(map(abs, dif[1:]))
```

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

