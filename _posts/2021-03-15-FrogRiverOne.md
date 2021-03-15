---
title: "[Codility] Lesson04 - Counting Elements(FrogRiverOne)"
category: Algorithm
tag: ["Python", "Codility", "Algorithm"]
---
오늘은 Codility 사이트 Lesson4. Counting Elements 에 있는 FrogRiverOne 문제를 풀어보자.

## 문제

 - 목적 : "Find the earliest time when a frog can jump to the other side of a river."

FrogRiverOne 문제는 개구리가 강을 건널 수 있는 최소 시간을 구하는 문제야.

개구리는 강의 한 쪽 끝 둑에서 반대쪽 둑까지 물에 떨어진 낙엽을 밟으면서 지나갈 수 있어. 개구리가 있는 둑의 위치를 0, 반대쪽 둑의 위치를 X+1 라고 하자.

낙엽은 1초에 한개씩 강 위에 떨어지며, 물 위 (1,2,...,X) 에 모두 떨어진 순간 개구리는 강을 건널 수 있어.

문제에서는 강의 길이 X 와 리스트 A 가 주어지고 (A[k] 는 k 초에 떨어진 잎의 위치), 몇 초에서 1~X 까지 잎이 모두 떨어져있는지 return 하면 돼.

예를들어, X = 5 이고 A = [1, 1, 3, 5, 2, 5, 4, 1, 3, 5] 인 경우, k = 6 이 되는 순간 1~5 까지가 모두 나오므로 6 을 return 해야해.

단, A 에 1 ~ X 중 하나라도 들어있지 않으면 -1 을 return 한다.

## 코드

 - 입력 : X (Integer), A (array whose elements is in the range [1..X])
 - 출력 : Integer(The earliest time when a frog can jump to the other side of a river)
 
```python

```

 - 코드 time complexity : O(N)


## 더 느린 코드


 - 코드 time complexity : O(N^2)
