---
title: "[Codility] Lesson01 - Iteration(BinaryGab)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---

오늘은 Codility 사이트에 있는 Lesson1.Iteration 에 있는 BinaryGab 문제를 풀어보자.

## 문제

 - 목적 : **"Find longest sequence of zeros in binary representation of an integer."** 

BinaryGab 문제는 양수 N 을 이진수로 표현할 때, 두 개의 1 사이에 연속적으로 차있는 0 의 갯수 중 가장 큰 값을 구하는 문제야.

예를들어, 숫자 9 의 경우 이진수로 표현하면 1001 이 되고 BinaryGab 은 2 가 돼. 숫자 529 는 이진수로 1000010001 이며 BinaryGab 은 4 가 돼.

만약 BinaryGab 이 없는 경우(ex. 1111)에는 0 을 반환해야해. 

## 코드

 - 입력 : N (Positive integer)
 - 출력 : BinaryGab (Non-negative interger)

```python
def solution(N):
    
    # 숫자 N 을 binary 로 변환
    bin_N = bin(N)[2:]
    
    # bin_N 에서 1 인 index 를 추출 
    one_index = [index for index, val in enumerate(bin_N) if val == '1']
    
    # 만약 1 인 index 가 1개라면 (ex. 1000) -> return 0
    if len(one_index) == 1:
        return 0
    
    # 두 개의 1 사이의 0 의 갯수를 앞에서부터 세어나가고, 이 중에서 가장 큰 값을 return 함.
    return max([one_index[i+1]-one_index[i]-1 for i in range(len(one_index)-1)])

```
위의 코드 설명은 아래와 같아.

 1. 일단 N 을 받으면 이를 이진수로 변환합니다.
 2. 이진수에 1 인 자리의 index 를 구합니다.
 3. 2의 index 길이가 1인 경우, 1 은 하나밖에 없으므로 0 을 반환합니다.
 4. 2의 index 길이가 1이 아닌 경우, 앞에서부터 순차적으로 1이 있는 두 개의 index 를 빼서 0 의 개수를 구합니다.
 5. 4에서 가장 큰 값을 반환합니다.

## 예시

 1. N = 210 인 경우,
 2. bin_N = 11010010
 3. one_index = [0, 1, 3, 6]
 4. [one_index[i+1]-one_index[i]-1 for i in range(len(one_index)-1)] = [1-0-1, 3-1-1, 6-3-1] = [0, 1, 2]
 5. 위의 max 값은 2
