---
title: "딥러닝 기초 - Input scaling"
category: Deep learning
tag: ["Input scaling", "Normalization", "Standardization"]
---

Input 데이터를 scaling 하는 작업은 딥러닝 모델 학습에서 거의 필수적으로 포함되어있어.

어떤 목적에서 scaling 을 하는걸까?

---
## Scaling 방법 (Normalization, Standardization)

Input scaling 에는 normalization, standardization 두 가지 방법이 가징 많이 사용돼.

일단 두 scaling 방법에 대해 알아보자.

### Normalization

Normalization 은 변수 값을 0~1 사이로 바꾸는 방법이야.

$x_1,x_2,...,x_n$ 의 input 이 있을 때, normalized input $x_1^{*}, x_2^{*},..., x_n^{*}$ 은 다음과 같이 구해져.

 - $x^*_i = diag (U-L) (x_i -L)$
   + where $U=\max_i x_i, L =\min_i x_i$, 각각은 elementwise 연산

이렇게 하면 normalized input 은 최소값이 0, 최대값이 1 이 되도록 변환돼.

### Standardization
