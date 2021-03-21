---
title: Linear regression concept (1)
category: Linear regression
tag: Linear regression
---

## Linear regression

Linear regression 은 관심 있는 response $y$ 를 다른 요소들로 설명하기 위해 식을 만드는 방법 중 하나에요.

이런 목적의 여러 방법들이 있지만, 가장 기본적으로 쓰이는게 linear regression 이에요.

Linear regression 은 다른 변수들의 선형식으로 y 를 추정/예측하는 방법이에요.

y 를 $x_1, x_2, x_3, .., x_p$ 의 변수의 선형식으로 추정하는게 목적이죠.

선형식을 만들기 위해서는 각 변수의 계수(coefficient)가 필요하죠. 이 계수들은 주어진 데이터를 통해서 추정할 수 있어요.

그 방법에는 Least squares method, Maximum likelihood method 가 있어요.

이 중에서 Least squares method 가 가장 일반적으로 쓰이는 방법이에요.

이 방법은 Gauss 가 고안한 방법으로, **오차가 가장 작은 직선이 가장 좋은 추정식이다.** 라는 아이디어로 만들어졌어요.

이것을 식으로 표현하면 $L = \sum_i (y_i - X  \beta)^2$ 를 최소화 하는 $\beta$ 를 찾자는 것이고

실제로 $ \hat{\beta} = (X^\intercal X)^{-1} X^\intercal y $ 로 쉽게 해가 찾아져요.

위에서 $X$ 는 $X = [1, x_1, x_2,...,x_p]$ 인 행렬이에요.


파이썬에서 skict-learn 패키지를 써서 나온 해는 위의 식으로 나온 해에요.

### 1. 추정된 계수는 해당 변수의 partial effect로 해석된다.

이는 벡터 공간에서 LSE, 오차를 생각하면 조금 쉽게 이해할 수 있어요. 

LSE를 구하는 것은 $y$ 라는 벡터를 $(1, x_1, x_2, ..., x_p)$ 로 만들어지는 공간으로 사영시키는 것과 동일한 의미에요. 

$y$ 는 시키면 $(1, x_1, x_2, ..., x_p)$ 공간 내의 $\hat{\beta_0} + \hat{\beta_1} x_1 + ... + \hat{\beta_p}x_p$ 와 $\hat{\epsion}$ 의 합으로 표현되어요.

한가지 기억할 것은  $\hat{\epsion}$은 $(1, x_1, x_2, ..., x_p)$과 수직한다는 것이고, 이는 사영이라는 행위에서 나온 결과에요.

일단 위의 내용은 벡터에 대해 이해가 필요하므로, 지금 당장 이해가 안되더라도 간단히 넘어가는게 좋아요.

### 문제 A.2.

$y$ 를 $x_1, ..., x_3$ 에 대해 LSE를 구하면 다음의 식이 나와요.

$y = \hat{\beta_0} +\hat{\beta_1} x_1 +\hat{\beta_2} x_2 + \hat{\beta_3} x_3 +\hat{\epsilon}... (1)$.

$x_1$ 를 $x_2, x_3$ 에 대한 LSE를 구하면 다음의 식이 나와요.

$x_1 = \hat{\gamma_0} +\hat{\gamma_2} x_2 + \hat{\gamma_2} x_3 +\hat{\v}... (2)$.

이제 식 (1) 에 $x_1$ 을 식 (2)로 치환한다면 식 (1) 은 다음과 같아요.

$y = \hat{\beta_0} +\hat{\beta_1} (\hat{\gamma_0} +\hat{\gamma_2} x_2 + \hat{\gamma_2} x_3 +\hat{\v}) +\hat{\beta_2} x_2 + \hat{\beta_3} x_3 +\hat{\epsilon}... (3)$.

그럼 위의 $y$ 는 다음과 같이 정리될 수 있어요.

$y =  (\hat{\beta_0} + \hat{\beta_1}\hat{\gamma_0}) 1 +\hat{\beta_1} \hat{\v} + \hat{\epsilon'}...(4)$

뒤에 $\hat{\epsilon'}$ 은 식 (3)에서 $x_2, x_3, \hat{\epsilon}$ 으로 이루어진 부분을 표시한 거에요.

식 (4)에서 $\hat{\v}$ 는 $1$과 수직이고, $\hat{\epsilon'}$과도 수직이에요, 이는 $\hat{\v}$ 가 식 (2)에서 구해졌기 때문이죠.

일단 여기까지 왔으면 자세한 풀이는 아래와 같아요.


<a href="https://i.imgur.com/UO84mSm"><img src="https://i.imgur.com/UO84mSm.jpg" width="300px" title="source: imgur.com" /></a>

$\hat{\alpha_1} = \hat{\beta_1}$ 인 이유는 위와 같이 수식으로 확인할 수 있고, 실제로 분석을 실행해보아도 같은 결과를 얻을 수 있어요.

### 문제 A.3.

문제 A.2. 와 비슷하게 풀 수 있어요.
