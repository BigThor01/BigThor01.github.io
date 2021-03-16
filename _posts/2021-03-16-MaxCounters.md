---
title: "[Codility] Lesson04 - Counting Elements(MaxCounters)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---

오늘은 Codility 사이트 Lesson4. Counting Elements 에 있는 MaxCounters 문제를 풀어보자.

## 문제

 - 목적 : "Calculate the values of counters after applying all alternating operations: increase counter by 1; set value of all counters to current maximum."

MaxCounters 문제는 N 개의 counter 에 대해 두 가지 operation 을 순차적으로 적용한 결과를 return 하는 문제야.

길이가 N 인 counter 는 처음에는 0으로 차있고, 다음의 두 개 operation 이 적용될 수 있어.

 - increase(X) : counter X 를 1 증가
 - max counter : 가장 큰 값을 counter 로 모든 counter 를 채움.

Operation 은 길이 M 인 array A 안에 순차적으로 들어있어. A의 element 값에 따라 다음 operation 과 매칭되어있어.

  1. A[K] = X, such that 1 <= X <= N, then operation K is increase(X)
  2. A[K] = N+1 then operation K is max counter.

예를들어, N = 5 이고 A = [3, 4, 4, 6, 1] 라면 counters 는 다음과 같이 구해져.

(0,0,1,0,0) → (0,0,1,1,0) → (0,0,1,2,0) → (2,2,2,2,2) → (3,2,2,2,2)

그럼 우리는 마지막에 (3,2,2,2,2) 을 return 하면 돼.

## 코드

 - 입력 : N (Integer), A (Non empty array whose elements is in the range [1..N+1])
 - 출력 : Counters (Array of N integers) 

첫 번째 생각한 코드는 0 이 차있는 길이 N 의 counters, counters의 max 값을 넣는 max_count 를 만들고 A 를 순회하면서 연산을 수행하는 거야.

만약 A[K] = N+1 이라면 미리 저장된 max_count 로 채우고, 아니라면 해당 counter에 1을 더하는거지.

```python
def solution(N, A):
    # counters : result
    result = [0] * N
    max_count = 0

    for i in A:
        if i == N+1:
            result = [max_count] * N
        else:
            result[i-1] +=1
            if max_count < result[i-1]:
                max_count = result[i-1]
    return result
```

 - 코드 time complexity : O(N * M)

단, 위와 같이 실행하는 경우 A 의 모든 element가 N+1 인 경우에는 O(N * M)의 complexity 를 가져서 개선이 필요했어.

## 수정 코드

위와 같이 연속적으로 N+1 이 나오는 경우 연산이 필요 없기 때문에 이를 해결하기 위해, if 문을 지나가는 것을 저장할 flag 를 만들었어.


```python
def solution(N, A):

    result = [0] * N
    max_count = 0
    last_is_max = False
   
    for i in A:
        if (i == N+1) & (~last_is_max):
            result = [max_count] * N
            last_is_max = True
        elif i <= N:
            result[i-1] +=1
            max_count = max(max_count, result[i-1])
            last_is_max = False
            
    return result
```

 - 코드 time complexity : O(N + M)
