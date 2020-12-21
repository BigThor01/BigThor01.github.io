---
title: "딥러닝 기초 - 역전파 알고리즘(2)"
category: Deep learning
tag: ["Bagpropagation"]
---

지난 포스팅에서 딥러닝 네크워크에서 가중치의 gradient 를 구하기 위한 역전파 알고리즘에 대해 직관적인 이해를 지난 포스팅에 대해 이야기했어.

오늘은 다음 영상을 참고해서 수식을 통해 실제로 gradient 를 구해보는 작업에 대해 정리해볼께.

 - [역전파 미적분 / 심층 학습, 4장, 3Blue1Brown youtube](https://youtu.be/tIeHLnjs5U8)


자세한 내용을 보기 전에, 오늘 다룰 내용을 간략히 소개하고 시작할께.

Gradient 를 계산하기 위해서는 일단 학습 예제를 네트워크를 따라 출력까지 보내는 forward propagation 을 수행해서 뉴런들의 활성화 값을 구하게 돼. 

모든 활성화 값을 구하고 나면, cost 를 각각의 활성화 값으로 미분한 미분계수를 출력층 부터 거꾸로 올라가면서 구하게 돼. 
이렇게 거꾸로 올라가면서 계산을 하는 이유는어떤 층의 활성화값로 미분한 값을 구할 때, 그 다음 층의 활성화로 미분한 미분계수가 포함되기 때문이야.

위와 같이 역전파 알고리즘을 통해 모든 뉴런의 활성화의 미분을 구할 수 있고 이것을 활용하면 가중치의 미분계수 즉, gradient 를 구할 수 있게 돼.

Cost 에 대한 가중치의 gradient 는 가중치 변동이 cost 를 얼마나 움직이는지를 알려주는 수치야. 미분값이 크면 가중치를 조금만 변화시켜도 cost 가 많이 움직이고, 반대로 미분값이 작으면 cost 는 조금만 움직여.

이는 negative gradient 방향($-\nabla C$) 이 cost 가 가장 빨리 감소하는 방향이라는 사실과 연결해서 생각할 수 있어.

이제 간단한 예제를 통해 역전파 알고리즘을 적용해서 가중치의 gradient 를 구해보도록 하자.

---
## 간단한 네트워크의 gradient 계산

두 개의 hidden layer 를 갖고 각 층에 1개의 뉴런만을 갖는 네트워크에 하나의 학습 예제를 흘려보냈다고 하자.

이런 상황에서 cost 가 가중치 변화에 어떤 영향을 받는지, 즉 가중치에 대한 미분을 구해보자.

일단 마지막 2개 층부터 살펴볼까? 출력층을 $L$, 그 이전 층을 $L-1$ 이라고 해보자. 또한 이전 층에서 전달받은 값은 $z$, $z$ 를 활성화한 활성화값은 $a$, 가중치와 편향은 각각 $w$, $b$ 라고 쓸께.

<a href="https://i.imgur.com/kLab4qY"><img src="https://i.imgur.com/kLab4qY.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

출력층을 $L$ 이라고 표기했으므로, $z^{(L)}$ 은 $L-1$ 층에서 전달받은 값, $a^{(L)}$ 은 $z^{(L)}$ 을 활성화한 값이 돼. 

그럼 학습 예제에 대한 cost 함수와  $z^{(L)}$, $a^{(L)}$ 은 다음과 같이 표현할 수 있어.
 
 - $z^{(L)} = w^{(L)} a^{(L-1)} + b^{(L)}$
 - $a^{(L)} = \sigma (z^{(L)})$, $\sigma (.)$ 는 활성화함수
 - $C_0 = (a^{(L)} - y)^2$
 
위의 관계를 가지고 우리는 $C_0위$ 를 가중치 $w^{(L)}$ 로 미분한 $\frac{\partial C_0}{\partial w^{(L)}}$ 을 연쇄법칙을 통해 구할 수 있어.

$\frac{\partial C_0}{\partial w^{(L)}} = \frac{\partial z^{(L)}}{\partial w^{(L)}} \frac{\partial a^{(L)}}{\partial z^{(L)}} \frac{\partial C_0}{\partial a^{(L)}}$

$\rightarrow \frac{\partial C_0}{\partial w^{(L)}} = a^{(L-1)}\ \sigma ´ (z^{(L)})\ 2 ( a^{(L)} - y)$

식을 보면 가중치의 영향은 가중치가 연결된 이전 층 뉴런의 활성화 정도($a^{(L-1)}$), 출력값과 실제값의 차이($ a^{(L)} - y$) 에 비례한다는 사실을 알 수 있어.

사실 위의 식은 하나의 예제에 대해서만 미분값을 구해본 거고, 모든 예제에 대해서 구해보면 다음과 같이 쉽게 구할 수 있어 (그건 cost 함수는 각 예제의 cost 를 평균해서 구하기 때문이지)

$\frac{\partial C}{\partial w^{(L)}} = \sum_{i=0}^{n-1} \frac{\partial z_i^{(L)}}{\delta w^{(L)}} \frac{\partial a_i^{(L)}}{\partial z_i^{(L)}} \frac{\partial C}{\partial a_i^{(L)}}$


$\frac{\partial C_0}{\partial w^{(L)}}$ 은 구했으니, 그럼 그 이전의 $(L-2)$ 와 $(L-1)$ 사이의 가중치 $w^{(L-1)}$ 에 대한 미분값을 구해볼까?

위에서 한 것처럼 연쇄법칙을 통해 구하게 되면

$\frac{\partial C_0}{\partial w^{(L-1)}} = \frac{\partial z^{(L-1)}}{\partial w^{(L-1)}} \frac{\partial a^{(L-1)}}{\partial z^{(L-1)}} \frac{\partial C_0}{\partial a^{(L-1)}}$

$\rightarrow \frac{\partial C_0}{\partial w^{(L-1)}} = a^{(L-2)}\ \sigma ´ (z^{(L-1)})\ \frac{\partial C_0}{\partial a^{(L-1)}} $


아까 전에 $\frac{\partial C}{\partial w^{(L)}}$ 와 위의 $\frac{\partial C}{\partial w^{(L-1)}}$ 를 보면 $\frac{\partial C_0}{\partial a^{(L)}}$, $\frac{\partial C_0}{\partial a^{(L-1)}}$ 처럼 가중치가 연결된 이후 뉴런 활성화의 미분값이 포함됨을 알 수 있어.

$a^{(L-1)}\ \sigma´(z^{(L)})$, $a^{(L-2)}\ \sigma ´(z^{(L-1)})$ 은 네트워크를 통과하면서 쉽게 구해지는 값이므로 우리는 위의 활성화에 대한 미분값만 구하면 gradient 를 쉽게 구할 수 있겠지.

<a href="https://i.imgur.com/Jve1ss6"><img src="https://i.imgur.com/Jve1ss6.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

위를 보면 활성화에 대한 미분값은 그 전에 연산된 활성화의 미분값을 가지고 다음과 같이 재귀적 계산으로 구할 수 있어.

 - $ \frac{\partial C_0}{\partial a^{(L)}} = 2 (a^{(L)}- y)$
 - $ \frac{\partial C_0}{\partial a^{(L')}} = \frac{\partial z^{(L'+1)}}{\partial a^{(L')}} \frac{\partial a^{(L'+1)}}{\partial z^{(L'+1)}} \frac{\partial C_0}{\partial a^{(L'+1)}}$, $L'=0, ... , L-1$.
 
 
이렇게 뒤에서부터 활성화에 대한 미분값을 순차적으로 구할 수 있고, 이 것을 가지고 다시 가중치에 대한 gradient 를 구할 수 있는거야. 이렇게 구해가는 알고리즘이 바로 역전파 알고리즘이야.

## 조금 더 복잡한 형태 (한 층에 2개 이상의 뉴런)

위에서 설명한 것은 너무 간단한 예제가 아닌가 싶기도 할꺼야. 실제로 층의 뉴런이 늘어나게되면 미분계산이 더 어렵지 않냐는 질문도 할 수 있어.

하지만 사실 뉴런이 늘어난다고 해도 실제로 구하는 작업은 동일해. 한 번 봐볼까?

<a href="https://i.imgur.com/mskdA4y"><img src="https://i.imgur.com/mskdA4y.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

한 층에 여러 뉴런이 있으니까 이제는 $L-1$ 층의 $k$ 뉴런과 $L$ 층에 $j$ 뉴런을 연결하는 가중치를 $w_{jk}^{(L)}$ 라 표기하면 $\frac{\partial C_0}{\partial w_{jk}^{(L)}}$ 은 아래와 같이 구해져. 아까 봤던 것과 동일해.

$\frac{\partial C_0}{\partial w_{jk}^{(L)}} = \frac{\partial z_{j}^{(L)}}{\partial w_{jk}^{(L)}} \frac{\partial a_{j}^{(L)}}{\partial z_{j}^{(L)}} \frac{\partial C_0}{\partial a_{j}^{(L)}}$


<a href="https://i.imgur.com/x0TOg2Z"><img src="https://i.imgur.com/x0TOg2Z.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

이제 활성화에 대한 미분값을 구하면 아까와 간단한 예제와 비슷하게 구할 수 있어.

 - $ \frac{\partial C_0}{\partial a_{j}^{(L)}} = 2 (a_{j}^{(L)}- y_j) + \sum_{l \neq j} (a_{l}^{(L)}- y_l)^2$
 - $ \frac{\partial C_0}{\partial a_{j}^{(L')}} = \sum_{k=0}^{n_{L'-1}} \frac{\partial z_{k}^{(L'+1)}}{\partial a_{j}^{(L')}} \frac{\partial a_{k}^{(L'+1)}}{\partial z_{k}^{(L'+1)}} \frac{\partial C_0}{\partial a_{k}^{(L'+1)}}$, $L'=0, ... , L-1$.
 
아까와 마찬가지로 위와 같이 모든 활성화에 대한 미분값을 구하면, 뉴런이 아무리 늘어나도 동일한 방식으로 가중치의 미분, 즉 gradient 를 계산할 수 있어.
