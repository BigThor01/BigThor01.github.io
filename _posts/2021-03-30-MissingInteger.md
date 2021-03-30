---
title: "[Codility] Lesson04 - Counting Elements(MissingInteger)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---
오늘은 Codility 사이트 Lesson4. Counting Elements 에 있는 MissingInteger 문제를 풀어볼께요.

## 문제

 - 목적 : "Find the smallest positive integer that does not occur in a given sequence."

MissingInteger 문제는 N개 정수가 있는 array A 에 포함되지 않은 가장 큰 자연수를 구하는 문제에요.

예를들어, A = [1,3,6,4,1,2] 인 경우, 포함되어 있지 않은 가장 큰 자연수 5 를 return 하면 됩니다.

만약 A = [1,2,3] 인 경우 4, A = [-1,-3] 인 경우 1 을 return 하면 되죠.

## 코드

 - 입력 : A (array whose elements is an integer)
 - 출력 : Integer (Smallest positive integer that does not occur in A)

저는 A 를 sorting 한 다음, 하나하나 순회하면서 비어있는 자연수를 찾은 방식으로 문제를 풀었어요.

Sorting 은 time complexity O(N log N), 순회하면서 비교하는 것은 O(N) 으로 계산 복잡도가 높지 않다고 생각했어요.

```python
def solution(A):
    # A를 sorting
    A.sort()
    # result는 아직 나오지 않은 가장 작은 자연수
    result = 1
    # A 를 순회
    for i in A:
        # i가 0 이하라면 계속 진행
        if i <= 0:
            continue
        
        # i가 자연수인 경우
        else:
            # result < i 라면 result 가 포함되지 않은 가장 작은 자연수
            if result < i
                return result
            # result = i 라면 result 에 1 더하기
            else:
                result += 1
```

코드 설명은 다음과 같아요.

 1. A를 sorting 합니다.
 2. result는 아직 나오지 않은 가장 작은 자연수를 넣는 변수입니다.
 3. i는 A를 순회합니다.
 4. i <= 0 이라면 계속 순회합니자.
 5. i가 자연수라면
   + result = i 인 경우, result를 1 더해서 update 합니다.
   + result < i 인 경우, result를 return 합니다.


 - 코드 time complexity : O(N) or O(N log N)
