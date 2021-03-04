---
title: "[Codility] Lesson03 - Time Complexity(PermMissingElem)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---

오늘은 Codility 사이트 Lesson3. Time Complexity 에 있는 PermMissingElem 문제를 풀어보자.

## 문제

 - 목적 : "Find the missing element in a given permutation."

PermMissingElem 문제는 길이 N 의 array A 가 주어질 때, 조건에 맞는 element 를 return 하는 문제야.

주어진 A (길이 N) 안에는 1,...,N+1 의 정수가 중복 없이 들어가있고, 단 하나의 정수만 빠져있어. 빠져있는 정수를 찾는게 문제의 목적이지.

예를들어, A = [4, 1, 5, 3] 라면 1 에서 5 까지 정수 중에서 2 가 빠져있으니 2 를 return 하면 되는거지.

## 코드

 - 입력 : A (array consisting of N different integers in 1,...,N+1)
 - 출력 : Integer (missing integer)

A 의 길이가 N 인 경우, 1 에서 N+1 까지의 합을 구하고 거기에서 A 의 element 합을 빼면 빠져있는 정수를 알 수 있어.

```python
def solution(A):
    # N+1 을 구함
    max_val = len(A) + 1
    
    # 1에서 N+1 까지의 합을 구하고, sum(A) 를 뺌
    return int((max_val * (max_val + 1)/2) - sum(A))
```

코드 설명은 다음과 같아.

 1. A 의 길이 N 을 구하고, max_val 에 N+1 을 넣습니다.
 2. 1 에서 N+1 까지의 합 (max_val) * (max_val+1)/2 에서 sum(A) 를 뺍니다.
 3. 2 의 결과는 float 형태이므로 integer 로 변화하여 반환합니다.

 - 코드 time complexity : O(N) or O(N log(N))

## 예시

 1. A = [5, 4, 2, 1, 6] 인 경우, max_val = len(A) + 1 = 6
 2. (max_val) * (max_val+1)/2 - sum(A) = 21.0 - 18 = 3.0
 3. int(3.0) = 3 을 return

