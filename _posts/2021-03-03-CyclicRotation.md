---
title: "[Codility] Lesson02 - Arrays(CyclicRotation)"
category: Python Coding
tag: ["Python","Codility","Algorithm"]
---

오늘은 Codilty 사이트 Lesson2. Arrays 에 있는 CyclicRotation 문제를 풀어보자.

## 문제

 - 목적 : "Rotation an array to the right by a given number of steps."

CyclicRotation 문제는 주어진 Array 에 대해, element 들을 한칸씩 뒤로 옮기는 문제야.

예를들어, A = [3, 8, 9, 7, 6] 를 CyclicRotation 하면 맨 뒤의 6 은 맨 앞으로 오고 나머지는 한칸씩 뒤로 가서 [6, 3, 8, 9, 7] 이 되고, 만약 여기에서 한번 더 CyclicRotation 를 하면 [7, 6, 3, 8, 9] 이 돼.

우리는 CyclicRoation 횟수인 K 가 주어지면 그에 대응되는 적절한 return 을 구하면 돼. 

## 코드

 - 입력 : A (array consisting of N integer), K (the number of CyclicRotation)
   + N and K are integers within the range [0, ..., 100]
 - 출력 : B (array, result of K CyclicRotations)

이 문제를 처음 보았을 때, 같은 action 을 주어진 K 만큼 수행하니까 다음의 반복문을 사용해서 풀었어.

```python
def solution(A,K):
    for i in range(K):
        # Array 맨 뒤 element 를 맨 앞으로, 나머지를 한칸씩 뒤로
        A = [A[-1]] + A[:-1]
        # K 번 반복
    return A
```

위의 코드로 87 점을 받게 되었는데, 이유는 A = [] 인 경우에 문제가 생기는 거였어. 이 부분과 for 문이 K 에 비례해서 반복되는 부분을 수정해보았어.

## 코드 수정

```python
def solution(A, K):
    # A = [] 인 경우, A 를 return
    if len(A) == 0:
        return A
    # A != [] 인 경우
    
    K = K % len(A)  
    # K <- K 를 len(A) 로 나눈 나머지
    
    # A의 (len(A)-K) 번째부터 끝까지 + A의 (len(A)-K-1) 번째 까지
    return A[(len(A)-K):] + A[:(len(A)-K)]
```

위 코드 설명은 아래와 같아.

 1. A = [] 을 받으면 그대로 return A
 2. A!= [] 인 경우, K 를 A 의 길이로 나눈 나머지만큼만 CyclicRotation 하면 됩니다.
 3. K <- K % len(A) 를 넣고
 4. A의 (len(A)-K) 번째부터 끝까지 + A의 (len(A)-K-1) 번째 까지로 list 만들어서 return 합니다.

## 예시

 1. A = [5, 1, 2, 4, 1], K = 12 인 경우,
 2. K = 12 % 5 = 2
 3. A[(len(A)-K):] + A[:(len(A)-K)] = A[3:] + A[:3] = [4, 1] + [5, 1, 2]
 4. [4, 1, 5, 1, 2] 를 return



