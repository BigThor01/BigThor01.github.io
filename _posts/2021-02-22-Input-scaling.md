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

$x_1,x_2,...,x_n$ 의 input 이 있을 때, normalized input $x_1^{\*}, x_2^{\*},..., x_n^{\*}$ 은 다음과 같이 구해져.

 - $x^*_i = diag (U-L)^{-1} (x_i -L)$
   + where $U=\max_i x_i, L =\min_i x_i$, 각각은 elementwise 연산

이렇게 하면 normalized input 은 최소값이 0, 최대값이 1 이 되도록 변환돼.

### Standardization

Standardization 은 변수의 평균을 0, 분산을 1 로 바꾸는 방법이야.

Standardized input $\tilde{x}_1, \tilde{x}_2,..., \tilde{x}_n$ 은 다음과 같이 구할 수 있어.

 - $\tilde{x}_i = diag(S)^{-1} (x_i -\mu)$
   + where $\mu=\frac{1}{n} \sum_i x_i,\ S=\frac{1}{n} \sum_i (x_i-\mu)^2$, 각각은 elementwise 연산

---
## Input scaling 은 목적함수의 minimal 을 바꾸진 않는다.

쉬운 예시인 simple linear regression 문제를 생각해보면, input $x$ 를 scaling 하는 경우에 $R^2$, MSE 는 scaling 과 상관없이 항상 동일한 값을 가져.

Scaling 에 의해 바뀌는 것은 회귀식의 추정치뿐이며, 목적함수는 그저 원래 목적함수를
shift/stretch 해서 구해질 수 있지. 이는, 목적함수의 minimal 자체는 동일하다는 의미야.

