---
title: "딥러닝 기초 - Unstable gradient"
category: Deep learning
tag: ["Gradient","Unstable","Vanishing","Exploding"]
---

깊은 네트워크를 학습할 때, 학습을 방해하는 가장 큰 문제는 바로 **unstable gradient problem** 이야.

불안정한 그라디언트 문제가 발생하면 가중치와 편향이 제대로 학습되지 않아서 모델의 정확도가 낮아지게 돼.

Unstable gradient 문제는 깊은 층의 네트워크에서 구조적으로 발생할 가능성이 높으며, 해당 문제가 발생하면 층마다 완전히 다른 속도로 학습이 되어서 결국 제대로 네트워크가 제대로 학습되지 않게 돼...

그럼 이런 문제가 왜 발생하느냐. 그건 바로 앞쪽 층의 gradient 가 모든 뒷 층 term 들의 곱으로 산출되기 때문이야.

오늘은 딥러닝 네트워크의 gradient, 특히 앞 층의 gradient 가 어떠한 형태로 표현되는지와 이런 구조에 의해 발생할 수 있는 **unstable(vanishing/exploding) gradient** 에 대해 이야기해볼께.


---
## 간단한 네트워크에서 gradient

<a href="https://i.imgur.com/I8bhLpU"><img src="https://i.imgur.com/I8bhLpU.png" width="700px" title="source: imgur.com" /></a>_@Why are deep neural networks hard to train?
 by Nielsen_

위에 그림처럼 각 층에 1개의 뉴런만 존재하는 간단한 네트워크를 생각해보자. 네트워크의 각 요소에 대한 표기를 정리하고 시작해보자.

 - $l\ (l=0,1,2,3,4)$ 은 층의 번호를 나타내며, $0$ 은 입력층, $4$ 는 출력층
 - $w_l$ 은 $l-1$ 층과 $l$ 층을 연결하는 가중치, $b_l$ 은 $l$ 층 뉴런의 편향
 - $l$ 층 뉴런에 입력값 $z_l = w_l a_{l-1} + b_l$, 활성화값 $a_l = \sigma ( z_l)$

표기를 정리했으니 각 가중치/편향에 대한 gradient 를 구해보자. 이전에 포스팅한 것처럼 각 항의 gradient 는 연쇄법칙을 통해 구할 수 있어.

 - $\frac{\partial C}{\partial b_4} = \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$
 - $\frac{\partial C}{\partial b_3} = \sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$
 - $\frac{\partial C}{\partial b_2} = \sigma´ (z_2) \times  w_3 \times \sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$
 - $\frac{\partial C}{\partial b_1} = \sigma´ (z_1) \times  w_2 \times \sigma´ (z_2) \times  w_3 \times \sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$

 - $\frac{\partial C}{\partial w_4} = a_3 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$
 - $\frac{\partial C}{\partial w_3} = a_2 \times\sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$
 - $\frac{\partial C}{\partial w_2} = a_1 \times\sigma´ (z_2) \times  w_3 \times \sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$
 - $\frac{\partial C}{\partial w_1} = a_0 \times\sigma´ (z_1) \times  w_2 \times \sigma´ (z_2) \times  w_3 \times \sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$

위에 각 term 의 위치는 실제로 네트워크에서의 순서와 동일하게 작성 했어. 식에서 알 수 있는 사실은 뒤에 있는 요소에 포함된 term 들이 이전 요소에 동일하게 포함된다는 사실이야.

$\delta_l = \frac{\partial C}{\partial z_l}$ 이라고 표현하면 위의 gradient 는 다음과 같이 표현할 수 있지.

 - $\frac{\partial C}{\partial b_4} = \delta_4$, ..., $\frac{\partial C}{\partial b_1} = \delta_1$ 
 - $\frac{\partial C}{\partial w_4} = a_3 \times \delta_4$, ..., $\frac{\partial C}{\partial w_1} = a_0 \times \delta_1$ 

여기에서 $\delta$ 는 다음과 같이 표현할 수 있어.

 - $\delta_4 = \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$
 - $\delta_l = \sigma´ (z_l) \times w_{l+1} \times \delta_{l+1},\ l = 1,2,3$
 
## 복잡한 네트워크에서의 gradient

<a href="https://i.imgur.com/S129Dbi"><img src="https://i.imgur.com/S129Dbi.png" width="700px" title="source: imgur.com" /></a>_@Why are deep neural networks hard to train?
 by Nielsen_
 
이제 간단한 네트워크에서 복잡한 네트워크로 확장해서 gradient 를 구해보자. 사실 위에서의 표현이 벡터로 확장된 것 이외에는 동일한 포맷으로 전개할 수 있어.

구조가 확장되었으니, 표기를 조금 변경해보자.

 - $l\ (l=0,1,2,...,L)$ 은 층의 번호를 나타내며, $0$ 은 입력층, $L$ 는 출력층
 - $w^l$ 은 $l-1$ 층과 $l$ 층을 연결하는 가중치 행렬 ($w_{ij}^l$ 은 $l-1$ 층의 $j$ 뉴런과 $l$ 층의 $i$ 뉴런을 연결)
 - $b^l$ 은 $l$ 층 뉴런의 편향 벡터, $b_i^l$ 은 $l$ 층 뉴런의 편향
 - $l$ 층 뉴런에 입력값 $z^l = w^l a^{l-1} + b^l$, 활성화값 $a^l = \sigma ( z^l)$ ($\sigma$ 는 element-wise)
 - $\Sigma´ (z^l) = diag (\sigma´ (z^l))$ 

위의 표기를 이용해서, 기존에 스칼라로 표현했던 $\delta_l$ 을 벡터의 형태로 확장하면,

 - $\delta^L = \frac{\partial C}{\partial z^L} = \Sigma´ (z^L)\ (a^L -y)$
 - $\delta^l = \frac{\partial C}{\partial z^l} = \Sigma´ (z^l) \ (w^{l+1})^\intercal\ \delta^{l+1},\ l = 1,...,l-1$

이고, 이것을 이용해 가중치와 편향에 대한 미분을 구하면 다음과 같아.

 - $\frac{\partial C}{\partial b^l} = \delta^l$
 - $\frac{\partial C}{\partial w^l} = \delta^l \ (a^{l-1})^\intercal$
 
 
---
## Vanishing gradient 가 왜 발생하는가?

**Vanishing gradient 가 발생하는 이유를 확인**하기 위해, 간단한 네트워크에서의 gradient 를 하나 가져와보자.

$\frac{\partial C}{\partial b_1} = \delta_1 = \sigma´ (z_1) \times  w_2 \times \delta_2$
$\rightarrow  \sigma´ (z_1) \times  w_2 \times \sigma´ (z_2) \times  w_3 \times \sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$

위의 식은 $w_l \times \sigma´ (z_l)$ 의 곱으로 표현되어있는데, 이 안에 있는 $\sigma´ (z_l)$ 는 항상 $(0,1/4]$ 범위 안에서 존재해.

<a href="https://i.imgur.com/5zMEqeI"><img src="https://i.imgur.com/5zMEqeI.png" width="700px" title="source: imgur.com" /></a>_@Why are deep neural networks hard to train?
 by Nielsen_

보통 가중치/편향 초기화는 $N(0,1)$ 을 사용하므로, 보통 $\|w_i\| < 4$ 을 만족하게 돼 (실제로는 0.99994 확률로 만족).

그럼 거의 대부분 경우에 $\|w_i \sigma´ (z_i)\| < 1$ 을 만족한다는거지.

한 layer 씩 앞으로 갈수록 해당 term 이 곱해져서 결국 exponentially 작아지게 되고, 결국 네트워크의 앞에 있는 gradient 는 vanishing 하게 돼.


이제 복잡한 네트워크에서 동일한 현상을 설명해볼까?

가중치에 대한 미분은 편향에 대한 gradient 를 구할 수 있기 때문에, 편향에 대한 gradient 만 전개해보자.

$\frac{\partial C}{\partial b^l} = \Sigma´ (z^l) \ (w^{l+1})^\intercal\ \delta^{l+1}$

$\rightarrow \Sigma´ (z^l) \ (w^{l+1})^\intercal\ \Sigma´ (z^{l+1}) \ (w^{l+2})^\intercal\ ... \Sigma´ (z^{L-1}) \ (w^{L})^\intercal\ \Sigma´ (z^{L})\ (a^L -y)$

위의 식으로부터 $\frac{\partial C}{\partial b^l}$ 의 upper bound 를 구해보자.

이 때 $\|\|Ax\|\| \leq \|\|A\|\| \cdot \|\|x\|\|$, $\|\|AB\|\| \leq \|\|A\|\| \cdot \|\|B\|\|$ 부등식을 활용해서 구할 수 있어.

위에서의 행렬 norm 은 induced matrix norm ($||A|| = \sup_{||x||=1} Ax$) 이야.

$||\frac{\partial C}{\partial b^l} || = ||\Sigma´ (z^l) \ (w^{l+1})^\intercal ... \Sigma´ (z^{L-1}) \ (w^{L})^\intercal\ \Sigma´ (z^{L})\ (a^L -y)||$

$\leq ||\Sigma´ (z^l)|| \cdot ||(w^{l+1})^\intercal|| \cdot .... \cdot ||\Sigma´ (z^{L-1})|| \cdot ||(w^{L})^\intercal|| \cdot ||\Sigma´ (z^{L})|| \cdot ||(a^L -y)||$

$= \prod_{r=l}^L ||\Sigma´ (z^r)|| \cdot \prod_{r=l+1}^L ||(w^{r})^\intercal||  \cdot  ||(a^L -y)||$

여기에서 $\gamma = \sup \{\sigma´ (\alpha) : \alpha \in R\}$ 이라고 하면, $||A|| = \max (singular\ value)$ 이기 때문에 다음과 같은 식이 얻어져.

$||\frac{\partial C}{\partial b^l} || \leq \gamma^{L-l+1} \cdot \prod_{r=l+1}^{L} ||w^r|| \cdot ||a^L-y||$

Sigmoid 나 tanh 를 사용하는 경우, $\gamma \leq 1$ 이기 때문에 $l$ 이 작아질수록 점점 exponentially 작아지게 되는거야.

## Exploding gradient 가 발생하는 경우도 있다.

$\frac{\partial C}{\partial b_1} = \sigma´ (z_1) \times  w_2 \times \sigma´ (z_2) \times  w_3 \times \sigma´ (z_3) \times w_4 \times \sigma´ (z_4) \times \frac{\partial C}{\partial a_4}$

위 식에서 만약 $w_2 = w_3 = w_4 = 100$ 이고, $\sigma´ (z_l)$ 들이 모두 그렇게 작지 않다고 하자 (거의 $1/4$ 근처).

그렇다면 $\frac{\partial C}{\partial b_1}$ 는 뒤 layer 의 gradient 보다 더 커지게 되고, 만약 이런 식으로 더 앞 layer 까지 늘어난다면 gradient 가 폭발적으로 커질 수도 있어.

물론 많은 경우, exploding gradient 보다는 vanishing gradient 현상이 발생하게 돼. 왜냐면 실제로 $w$ 가 커지게 되면 이와 상충되게 $\sigma´$ 는 작아지고 거의 $0$ 에 가까워질 가능성이 높기 때문이지.

---
## 요약하면

Vanishing/exploding gradient 는 모두 앞 layer 가 뒤의 layer 에 term 들의 곱으로 표현되기 때문에 발생하는 문제야.

즉, 층에 따라 앞과 뒤에서 서로 다른 속도로 학습이 될 수 밖에 없는 구조로 인해 제대로 학습되지 않는 문제가 생기는 거야. 뒤에 있는 parameter 는 움직이지만 앞에 있는 것들은 거의 초기값에서 변화가 없는거지. 혹은 앞에 있는 parameter 가 크게 진동해서 수렴을 하지 않던가.

Sigmoid/tanh 를 사용하는 경우에는 미분값이 0~1 사이이므로 vanishing 현상이 일어나게 돼.

다음에는 이러한 구조적인 문제에서 발생하는 unstable gradient 를 해결하기 위한 방법을 소개할께.



