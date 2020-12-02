---
title: "[CNN by Andrew Ng-01] Computer vision & Convolution"
category: Convolutional Neural Networks
tag: ["Computer vision", "Convolution", "Edge dectection"]
---

\[CNN by Andrew Ng\] 포스트는 Andrew Ng 선생님의 CNN(Convolutional Neural Networks) 유투브 강의를 듣고 정리한 내용을 올릴 예정이야.

 - 강의 링크 : [Convolutional Neural Networks by Andrew Ng](https://www.youtube.com/watch?v=ArPaAX_PhIs&list=PLkDaE6sCZn6Gl29AoE31iwdVwSG-KnDzF)

---

## What is Computer Vision?

**Computer vision**은 **사람이나 동물의 시각 체계 기능을 컴퓨터에서 구현하는 기술** 을 의미해.

사람은 눈을 통해 보이는 사물을 구분하거나 전방의 사물을 인지할 수 있는 시각/인식 체계를 가지고 있어.

<a href="https://i.imgur.com/rHoB6Th"><img src="https://i.imgur.com/rHoB6Th.png" width="700px" title="source: imgur.com" /></a>

예를들어, 위의 사진이 주어지면 우리는 시각적으로 이미지를 확인하고 특성을 파악해서 "저건 고양이다."라고 판단할 수 있어.

Computer vision 기술은 예시에서의 인식 체계를 학습된 프로그램이 수행할 수 있게 만드는 목적을 가지고 있지. 

실제로 computer vision 기술을 통해 해결할 수 있는 문제들은 다음과 같아.

 - **Image classification**: 인식된 이미지에 labeling을 하는 것.
 - **Object detection**: 인식된 이미지에서 사물의 위치 및 모양을 탐지하는 것.
 - **Neural style trasfer**: 인식된 이미지를 다른 스타일로 재구성하는 것.

자율주행, 생체인식 등의 다양한 분야에서 활용될 수 있기 때문에 computer vision 기술은 나날이 발전하고 있어.


우리는 많은 computer vision 기술 중에 가장 널리 알려진 **CNN(Convolutional Neural Networks)**에 대해 공부할 예정이야.

오늘은 CNN 구조에 핵심 연산인 **convolution(합성곱)** 연산에 대한 개념 및 해당 연산이 어떤 의미를 갖는지 설명해볼께.

---
## Convolution operation(합성곱 연산) 

세로선이 많은 이미지.
<a href="https://i.imgur.com/rHoB6Th"><img src="https://i.imgur.com/rHoB6Th.png" width="700px" title="source: imgur.com" /></a>

위의 이미지를 보면 사람도 있고, 난간 또는 자전거 등의 여러 특성을 가진 개체들이 있어.

다르게 말하면, 위의 이미지에는 수많은 정보들이 포함되어 있어.

 - 바퀴의 모양 및 방향
 - 사람의 모양 및 방향
 - ...

여기서 하나의 질문을 해볼까?

Q. 위의 이미지에서 세로 윤곽선이 어디에 있는지 알려면 어떻게 해야할까?

이미지에서 윤곽선을 추출하는 것을 **edge detection**이라고 하고, 위의 질문은 그 중에서도 **vertical edge detection** 문제야.

위의 질문은 이미지에 포함된 수많은 정보 중에서 세로 윤곽선 정보만을 추출하는 방법에 대한 질문이고, 이러한 방법은 해당 정보가 중요한 경우 다른 불필요한 정보를 없애기 위해 사용할 수 있지.

그럼 어떤 방법으로 세로 운곽선을 검출할 수 있는지 간단한 예시를 통해 살펴보자.


### Edge detection(윤곽선 검출)

임의의 $6 \times 6$ 픽셀의 grayscale 이미지가 있다고 하자. 해당 이미지는 픽셀의 음영에 따라 값을 갖는 $6\times 6$ 행렬로 표현이 가능해.


    \begin{matrix}
    1 & x & x^2 \\
    1 & y & y^2 \\
    1 & z & z^2 \\
    \end{matrix}

