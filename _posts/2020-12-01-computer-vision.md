---
title: "[CNN by Andrew Ng-01] Computer vision & Convolution"
category: Convolutional Neural Networks
tag: ["Computer vision", "Convolution", "Edge dectection"]
---

\[CNN by Andrew Ng\] 포스트는 Andrew Ng 선생님의 CNN(Convolutional Neural Networks) 유투브 강의를 듣고 정리한 내용을 올릴 예정이야.

 - 강의 링크 : [Convolutional Neural Networks by Andrew Ng](https://www.youtube.com/watch?v=ArPaAX_PhIs&list=PLkDaE6sCZn6Gl29AoE31iwdVwSG-KnDzF)

---

## What is Computer Vision?

**Computer vision**은 **사람이나 동물의 시각 체계의 기능을 컴퓨터에서 구현하는 기술**을 의미해.

사람은 눈을 통해 보이는 사물을 구분하거나 전방의 사물을 인지할 수 있는 시각/인식 체계를 가지고 있어.

예를들어, 내 앞에 고양이가 지나가면 시각적으로 이미지를 보고 특성을 파악해서 "저건 고양이다."라고 판단할 수 있어.

Computer vision 기술은 예시에서의 인식 체계를 학습된 프로그램이 수행할 수 있게 만드는 목적을 가지고 있지. 


실제로 computer vision 기술을 통해 해결할 수 있는 문제들은 다음과 같아.

 - **Image classification**: 인식된 이미지에 labeling을 하는 것.
 - **Object detection**: 인식된 이미지에서 사물의 위치 및 모양을 탐지하는 것.
 - **Neural style trasfer**: 인식된 이미지를 다른 스타일로 재구성하는 것.

자율주행, 생체인식 등의 다양한 분야에서 활용될 수 있기 때문에 computer vision 기술은 나날이 발전하고 있어.



우리는 많은 computer vision 기술 중에 가장 널리 알려진 **CNN(Convolutional Neural Networks)**에 대해 공부할 예정이야.

오늘은 CNN 구조의 핵심이 되는 **convolution(합성곱)** 연산에 대한 개념을 설명해볼께.



## Convolution operation(합성곱 연산) 

