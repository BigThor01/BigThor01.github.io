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

위에 각 term 의 위치는 실제로 네트워크에서의 순서와 동일한 순서로 작성 했어. 위에서 보면 공통적으로 다음의 term 이 들어감을 알 수 있어.

 - $w_j \sigma´ (z_j)$, 사실 얘는 $\frac{\partial a_j}{\partial a_{j-1}}$ !
 
