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

쉬운 예시인 simple linear regression 문제를 생각해보면, input $x$ 를 scaling 하는 경우에 $R^2$,$MSE$ 는 scaling 과 상관없이 항상 동일한 값을 가져.

Scaling 에 의해 바뀌는 것은 회귀식의 추정치뿐이며, 목적함수는 그저 원래 목적함수를 shift/stretch 해서 구해질 수 있지. 이는, 목적함수의 minimal 자체는 동일하다는 의미야.

그럼 neural network 에서는 어떨까? Scaling 했을 때, 목적함수의 minimal 은 변할까?

### Neural network 목적함수

<a href="https://i.imgur.com/S129Dbi"><img src="https://i.imgur.com/S129Dbi.png" width="700px" title="source:imgur.com"/></a>_@by Nielsen_

위와 같은 neural network 를 생각해보자. 네트워크 학습시에 목적함수는 실제값과 예측값 차이를 측정하는 비용함수 $L$ 로 정의돼.

네트워크를 $F$, 손실함수를 $l$ 이라 하면 목적함수는 다음과 같아.

$L = \sum_i l(F(x_i, \theta),y_i)$, 여기에서 $\theta$ 는 네트워크에서 학습할 parameter 를 의미해.

Input scaling 효과를 살펴보기 위해서, 목적함수를 다르게 표현해보자.

첫 번째 hidden layer 까지를 $F_1$, 그 이후를 $F_2$ 로 나누어 표현하면 목적함수는 다음과 같아.

$L(W_1, b_1, \theta_2) = \sum_i l(F_2(F_1(x_i, W_1,b_1), \theta_2), y_i)$ (식 $(1)$)

### Input scaling 은 목적함수를 stretch/shift 만 한다.

Scaling 한 input 을 $\tilde{x}$ 라 하고, 이를 사용해서 만든 목적함수를 $\tilde{L}$ 라고 하자.

$(1)$ 을 참고하면, $\tilde{L}$ 은 다음과 같아.

$\tilde{L} (W_1, b_1, \theta_2) = \sum_i l(F_2(F_1(\tilde{x}_i, W_1,b_1), \theta_2), y_i)$ (식 $(2)$)

$F_1(\tilde{x}_i, W_1, b_1) = \sigma (W_1 \tilde{x}_i + b_1)$ 이고 $\tilde{x}_i$ 는 $x_i$ 의 선형변환이라는 사실을 이용하면,

식 $(2)$ 는 다음과 같아.

$\tilde{L} (W_1, b_1, \theta_2) = \sum_i l(F_2(\sigma(W_1 \tilde{x}_i + b_1), \theta_2), y_i)$ 

$ = \sum_i l(F_2(\sigma(W_1 (A x_i + c) + b_1), \theta_2), y_i)$

$ = \sum_i l(F_2(\sigma(W_1 A x_i + W_1 c + b_1), \theta_2), y_i)$

$ = L(W_1 A, W_1c + b_1, \theta_2)$, 여기에서 $A$, $c$ 는 선형변환에 대응되는 행렬과 벡터

위의 결과는, input scaling 해서 만든 $\tilde{L}$ 은 기존의 목적함수 $L$ 을 $w_1, b_1$ 방향으로 stretch/shift 한 것임을 의미해.

---
## Input scaling 은 학습 속도 개선에 효과가 있다.

위에서 확인한 것처럼 input scaling 은 첫번째 층의 parameter 방향으로 shift/stretch 하는건데, 이런 작업은 학습 속도 개선에 효과가 있어.

복잡한 네트워크에서 input scaling 이 정확히 어떻게 working 하는지 확인하기는 어렵기 때문에, 두 가지 간접적인 방법으로 확인해볼꺼야.

### Input scaling 은 gradient 방향을 최적점으로 향하게 한다.

<a href="https://www.allaboutcircuits.com/uploads/articles/how-to-perform-classification-using-a-neural-network-introducing-the-perceptron_RK_AAC_image1"><img src="https://www.allaboutcircuits.com/uploads/articles/how-to-perform-classification-using-a-neural-network-introducing-the-perceptron_RK_AAC_image1.jpg" width="650px" title="source:imgur.com"/></a>

첫번째로 위와 같이 입력/출력 층으로만 이루어진 네트워크를 생각해보자. 목적함수 $L$ 은 다음과 같아.

$L(w_1, w_2, b)= \sum_i l(\sigma(w^\intercal x_i+b), y_i)$ (식 $(3)$)

문제를 간단하게 하는 몇 가지 가정을 하고 input scaling 효과를 살펴보도록 할께.

 - 손실함수 $l$ 은 $l_2$ loss 를 사용
 - activation $\sigma$ 는 identity 를 사용
 - 편향 $b$ 는 $b=\bar{y} - w^\intercal \bar{x}$ 를 사용

그럼 식(3)은 다음과 같이 전개할 수 있어.

$L(w_1,w_2) = \sum_i \|\|\tilde{y}_i -w^\intercal \tilde{x}_i \|\| ^2$

$= w^\intercal (\sum_i \tilde{x}_i \tilde{x}_i^\intercal )w - 2 (\sum_i \tilde{y}_i \tilde{x}_i^\intercal ) w + C$

$= \|\| (\sum_i \tilde{x}_i \tilde{x}_i^\intercal )^{\frac{1}{2}} w + v \|\| ^2 + C'$, 여기에서 $\tilde{x}_i = x_i -\bar{x},\ \tilde{y}_i = y_i -\bar{y}$ 이고, $v,C,C'$ 는 상수를 의미해.

위의 형태를 보면, 목적함수 $L$ 의 등고선은 타원의 형태를 가지며 타원의 장축과 단축은 $x$ 의 분산-공분산 행렬인 $(\sum_i \tilde{x}_i \tilde{x}_i^\intercal )$ 의 eigenvalue 값과 관계가 있어.

타원의 장축은 $\min (eigenvalue)$, 단축은 $\max (eigenvalue)$ 와 반비례해.

$x_1,x_2$ 의 산포 차이가 커질수록 두 egienvalue 차이는 커지며, 그러므로 타원은 더 찌그러진 형태가 돼.

<a href="https://www.jeremyjordan.me/content/images/2018/01/Screen-Shot-2018-01-23-at-2.27.20-PM"><img src="https://www.jeremyjordan.me/content/images/2018/01/Screen-Shot-2018-01-23-at-2.27.20-PM.png" width="700px" title="source:imgur.com"/></a>

위에 등고선을 표현한 그림을 보면, 등고선이 타원에서 원에 가까워질수록 gradient 방향이 최적점을 향함을 알 수 있어. 

결과적으로 input scaling 을 하는 경우, 목적함수의 gradient 방향이 최적점으로 향하게 되면서 더 빠른 속도로 수렴할 수 있는거지.
### 실제로 네트워크 학습 속도를 비교해보자.

두번째로 일반적인 네트워크에서 실제 학습 속도에 어떤 차이가 있는지 실험해보자.




