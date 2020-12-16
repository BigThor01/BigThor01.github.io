---
title: "딥러닝 기초 - 역전파 알고리즘(1)"
category: Deep learning
tag: ["Bagpropagation"]
---

딥러닝에서 네트워크를 학습하는 핵심 알고리즘인 **역전파(backpropagation)**에 대해 알아보자.

"역전파 알고리즘(1)" 포스팅에서는 기존에 알고있는 내용을 간략히 정리하고, 역전파 알고리즘에 대해 공식을 배제한 직관적으로 이해를 해보자.

오늘 정리하는 내용은 다음의 영상을 참고해서 작성했어.

 - [3Blue1Brown youtube](https://youtu.be/Ilg3gGewQ5U)

---
## Recap

숫자를 손으로 쓴 MNIST 예제를 통해 신경망에 대해 간략히 정리해보자. MNIST 데이터셋에서 학습 예제는 $28\times 28$ 손으로 쓴 숫자 이미지와 정답 레이블로 구성되어있어.

<a href="https://i.imgur.com/LztChQB"><img src="https://i.imgur.com/LztChQB.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

숫자 이미지를 분류하기 위한 네트워크는 $28 \times 28 = 784$ 뉴런의 **입력 layer**, 10개 클래스 뉴런의 **출력 layer**, 그리고 중간을 연결하는 **hidden layer**로 구성되며,
각 층간에는 **가중치(weight)**와 **편향(bias)**으로 연결되어있어. 

이 네트워크는 여러 예제를 학습하면서, 네트워크가 숫자 이미지를 더 잘 분류하는 방향으로 가중치와 편향을 조정하게 돼.

**학습(learning)**은 미리 정의된 **cost 함수**를 줄이는 방향으로 가중치와 편향을 조정해가는 **경사하강법(gradient descent)**을 통해 이루어져.

<a href="https://i.imgur.com/RSG4K1l"><img src="https://i.imgur.com/RSG4K1l.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

각 학습 예제의 cost는 네트워크를 통과한 산출값과 실제 레이블값의 차이를 표현하도록 다양하게 정의할 수 있는데, 이번 포스팅에선 오차제곱을 사용할꺼야.

모든 학습 예제에 대해서 cost를 구해서 평균을 구하게 되면, 이게 바로 네트워크의 cost 야.

<a href="https://i.imgur.com/He5X2mL"><img src="https://i.imgur.com/He5X2mL.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

우리가 찾고 있는 것은 cost 함수의 **음의 기울기(negative gradient)** 방향이고, 그 방향으로 가중치와 편향을 반복적으로 조정해가면서 cost를 가장 효율적으로 줄여나갈 수 있어.

이러한 방식으로 최적해를 찾는 방법이 경사하강법이며, cost 함수의 gradient가 복잡한 형태이며고 최적해를 closed form으로 구할 수 없기 때문에 이러한 numerical한 방법을 통해 구할 수 밖에 없어.

여기서 잠깐, cost 함수의 gradient의 의미에 대해 잠깐 알아보도록 할까?

Negative gradient ($-\nabla C$) 는 cost 함수가 각 가중치와 편향의 변화에 얼마나 민감하게 움직이는지를 나타낸다고 생각할 수 있어.

<a href="https://i.imgur.com/Ldsv6iH"><img src="https://i.imgur.com/Ldsv6iH.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

위의 그림을 보면, 미분값이 3.2 인 가중치가 0.1 인 가중치보다 cost를 32배 더 민감하게 변화시키는 가중치라는 것을 의미해.

또한, 어떤 특정한 점에서 negative gradient는 함수를 가장 빠르게 감소시키는 방향을 의미하기도 해. Gradient에 대한 자세한 내용은 최적화와 관련된 포스팅으로 자세하게 설명하도록 할께.


## Intuitive walkthrough

위에서 이야기한 내용은 **"분류를 잘하는 네트워크를 만들기 위해, gradient descent 방법을 통해 가중치와 편향을 조정하면서 최적해를 찾는 것이 학습이다"** 라고 요약할 수 있어.

다만, cost 는 수많은 가중치와 편향의 함수이기 때문에 무작정 gradient를 계산하는 것은 복잡하고 헷갈리는 일이 될 것 같아. 이를 효율적으로 계산하는 방법이 바로 **역전파(backpropagation) 알고리즘** 이야. 

역전파 알고리즘은 딥러닝을 공부하면 자연스럽게 여러 번 접하게 되는데, 그때마다 수많은 지수 및 첨자 표현으로 혼란스러울 때가 많았어.

사실 이런 기호들은 단지 단계적인 가중치/편향을 표현하기 위한 단순한 표기들인데 말이야.

일단, 우리는 이런 복잡한 표기들은 다 지우고 역전파 알고리즘이 무엇인지를 직관적으로 이해해보자.

### 역전파 알고리즘

숫자 2 이미지의 한 가지 훈련 예제를 가지고 역전파 알고리즘을 이해해보자. 사실 어떤 이미지에 대해서도 동일한 과정이 적용되므로 한 가지 예제만 살펴보면 돼.

하나의 이미지가 가중치 조정에 어떤 영향을 미칠까?

<a href="https://i.imgur.com/k5I7nDX"><img src="https://i.imgur.com/k5I7nDX.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

위의 네트워크는 덜 학습된 네트워크야. 위와 같이 숫자 2의 이미지를 흘려보내면 출력 층 결과는 위와 같이 다소 랜덤해보여.

그럼 여기에서 cost 를 줄이기 위해 어떻게 해야할까?

처음으로 생각할 수 있는 것은 출력 층 뉴런을 조정하는 방법이야. 출력 층 3번째 뉴런(숫자 2)를 보면 출력값과 실제값의 차이가 크고 9번째 뉴런(숫자 8)의 출력값과 실제값의 차이가 작아.

이런 경우, 3번째 뉴런을 조정하는 것이 9번째 뉴런을 조정하는 것보다 cost 를 더 많이 조정할 수 있어. 이는 9번째 뉴런보다 3번째 뉴런 활성화값을 조정해야함을 의미해.


<a href="https://i.imgur.com/G3naiBa"><img src="https://i.imgur.com/G3naiBa.png" width="700px" title="source: imgur.com" /></a>_@3Blue1Brown_

그럼 3번째 뉴런의 활성화값은 어떻게 바꿀 수 있을까? 해당 뉴런은 이전 층의 뉴런들과 연결되어있고 우리는 다음 세 가지 액션으로 조정 가능해.

 - 편향 $b$ 를 조정
 - 가중치 $w_i$ 를 조정
 - 활성화 $a_i$ 를 조정





