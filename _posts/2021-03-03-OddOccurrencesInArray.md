---
title: "[Codility] Lesson02 - Arrays(OddOccurrencesInArray)"
category: Algorithm
tag: ["Python","Codility","Algorithm"]
---

오늘은 Codilty 사이트 Lesson2. Arrays 에 있는 OddOccurrencesInArray 문제를 풀어보자.

## 문제

 - 목적 : "Find value that occurs in odd number of elements."

OddOccurrencesInArray 문제는 일단 non-empty array A 가 주어질 때, 조건에 맞는 element 를 return 하는 문제야.

주어진 A 는 홀수 N 개의 element 로 이루어진 array 인데, 그 중 1개 element 를 제외하고 모두 각자 같은 값을 갖는 짝을 가지고 있어.

짝이 없는 1개의 element 를 찾는게 문제의 목적이야.

예를들어, A = [9, 3, 9, 3, 9, 7, 9] 라면 짝이 없는 7 을 return 하면 되는거지.

## 코드

 - 입력 : A (array consisting of N odd numbers)
 - 출력 : One element that is left unpaired (integer)

일단 A 를 순회하면서 element 들의 count 를 세고, count 가 홀수인 element 를 return 하는 방식으로 풀었어.

```python
def solution(A):
    count = dict()
    
    # element 를 key, count 를 value 로 생성
    for i in A:
        count[i] = count.get(i,0) + 1
    # count 가 홀수인 경우, 해당 element 를 return
    for key, value in count.items():
        if value % 2 == 1:
            return key
```

코드 설명은 다음과 같아.

 1. A를 받으면, 각 element의 count를 저장할 dictionary를 만듭니다.
 2. A를 순회하면서, element 값을 key로 count를 value로 저장합니다.
 3. 만들어진 dictionary에서 value가 홀수인 key를 반환합니다.

 - 코드 time complexity : O(N) or O(N logN)

## 참고 코드

문제를 XOR 연산으로 푼 코드가 있어서 소개하도록 할께.

```python
from functools import reduce

def solution(A):
    # A를 순회하면서 XOR 연산을 수행
    return reduce(lambda x,y: x^y, A)
```

XOR 연산은 연산 순서에 관계 없으므로 코드를 수행하면, 짝이 있는 element는 0 이 되고 결과적으로 짝이 없는 element만 남게 되는거지.

 - 코드 time complexity : O(N) or O(N logN)
