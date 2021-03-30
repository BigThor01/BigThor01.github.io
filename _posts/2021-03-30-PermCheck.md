---
title: "[Codility] Lesson04 - Counting Elements(PermCheck)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---
오늘은 Codility 사이트 Lesson4. Counting Elements 에 있는 PermCheck 문제를 풀어볼께요.

## 문제

 - 목적 : "Check whether array A is a permutation."

PermCheck 문제는 N개 정수가 있는 array A 가 주어질 때, 해당 array가 permutation 인지 판단하는 문제에요.

길이가 N인 array A 에 1~N 까지 중복되는 숫자가 없는 경우 permutation 이라고 해요. 주어진 A 가 permutation 인 경우 1을 return, 아닌 경우 0을 return 하면 돼요.

예를들어, A = [1,3,4,2] 인 경우 A 는 permutation 이므로 1을 return 하면 됩니다. A = [4,1,3] 인 경우 permutation 이 아니므로 0을 return 하면 되고요.

## 코드

 - 입력 : A (Array whose elements is a positive integer)
 - 출력 : Integer (whether A is a permutation)

저는 A 를 sorting 한 다음, 하나하나 순회하면서 비어있는 자연수가 있는지 찾은 방식으로 풀었어요.

Sorting 은 time complexity O(N log N), 순회하면서 비교하는 것은 O(N) 으로 계산 복잡도가 높지 않다고 생각했어요.

```python
def solution(A):
    # A를 정렬
    A.sort()
    # value 는 아직 나오지 않은 가장 작은 자연수
    value = 1
    # A를 순회
    for i in A:
        # value == i 라면 계속 진행
        if value == i:
            # value == i 이면 1을 증가
            value += 1
            continue
        else:
            # value != i 라면 return 0
            return 0
    # A를 다 순회하면 return 1
    return 1
```

코드 설명은 다음과 같아요.

 1. A를 sorting 합니다.
 2. value는 아직 나오지 않은 가장 작은 자연수를 넣는 변수입니다.
 3. i는 A를 순회합니다.
   - i  == value 라면
       + value에 1 더해서 update 합니다.
       + 다음 순회를 진행합니다.
   - i != value 라면
       + 0을 return 합니다.
 4. 끝까지 순회하면 1을 return 합니다.


 - 코드 time complexity : O(N) or O(N log N)
