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

우리가 찾고 있는 것은 cost 함수의 **음의 기울기(negative gradient)** 방향이고, 그 방향으로 가중치와 편향을 반복적으로 조정해가는 cost를 가장 효율적으로 줄여나갈 수 있어.


