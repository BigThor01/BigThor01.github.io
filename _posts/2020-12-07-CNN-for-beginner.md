---
title: "Convolutional Neural Network 첫걸음 (1)"
category: Computer vision
tag: ["Convolutional Neural Network", "Convolution"]
---

Computer vision 업무를 진행하면서 알게된 내용을 꾸준히 포스팅하려해.

**"Convolutional Neural Network 첫걸음"** 포스트는 **합성곱 신경망(Convolutional Neural Net)**을 처음 접한 초보자에게 개요를 설명하는 포스트야.

해당 포스트는 [Adit Deshpande's blog](https://adeshpande3.github.io/adeshpande3.github.io/A-Beginner's-Guide-To-Understanding-Convolutional-Neural-Networks/)를 참고해서 작성했어.

오늘 다룰 주제는 다음의 두 가지.

 - Computer vision 이란 무엇인가?
 - 합성곱 신경망 outline

추가적인 참고 자료는 다음과 같아.

- [Matthew D.Zeiler and Rob Fergus, 2014](https://cs.nyu.edu/~fergus/papers/zeilerECCV2014.pdf)
- [Visualizing and Understanding Deep Neural Networks by Matt Zeiler](https://www.youtube.com/watch?v=ghEmQSxT6tw)
- [Jason Yosinski's youtube](https://www.youtube.com/watch?v=AgkfIQ4IGaM&t=155s)
- [Otavio Good's youtube](https://www.youtube.com/watch?v=f0t-OCG79-U)

---
## Computer vision 이란 무엇인가?

Computer vision 은 사람이나 동물의 시각 체계 기능을 컴퓨터에서 구현하는 기술을 의미해.

사람은 눈을 통해 보이는 사물을 구분하거나 전방의 사물을 인지하는 시각/인식 체계를 가지고 있어.

<a href="https://i.imgur.com/hSAZCZH"><img src="https://i.imgur.com/hSAZCZH.jpg" width="700px" title="source: imgur.com"  caption="고양이"/></a>

예를들어, 위의 사진이 주어지면 사람은 얼굴 모양, 눈, 귀, 코, 발 등의 특성을 파악하고 "고양이"라고 판단할 수 있어.

Computer vision 기술의 목적은 이러한 사람의 훌륭한 인식 체계를 프로그램이 수행할 수 있게 만드는 목적을 가지고 있어.

Computer vision 기술을 통해 해결할 수 있는 문제들은 다음과 같아.

 - Image classification : 이미지의 class를 맞추는 문제
 - Object detection : 이미지에서 사물의 위치 및 모양을 탐지하는 문제
 - Neural style transfer : 원본 이미지를 다른 스타일로 재구성하는 문제
 
 자율주행, 생체인식 등의 다양한 분야에서 활용될 수 있기 때문에 computer vision 기술은 나날이 발전하고 있어.
 

---
## 합성공 신경망(Convolutional Neural Net) Outline

Computer vision 분야를 발전시킨 가장 혁신적인 방법 중 하나인 **합성곱 신경망**에 대해 이야기해보자.

입력된 input 이미지가 어떤 class에 속하는지 분류하는 작업을 **image classification** 이라 불러.

사람은 이런 작업을 큰 노력 없이 다양한 변형에서도 빠르고 정확하게 할 수 있어. (정말 놀라운 일이야!)

Hubel and Wiesel(1962)는 사람이 이미지를 인식할 때, 일부 뉴런 세포가 특정한 방향의 윤곽선에서만 반응한다는 사실을 알아냈지.


**Computer vision**은 **사람이나 동물의 시각 체계 기능을 컴퓨터에서 구현하는 기술** 을 의미해.

사람은 눈을 통해 보이는 사물을 구분하거나 전방의 사물을 인지할 수 있는 시각/인식 체계를 가지고 있어.

<a href="https://i.imgur.com/rHoB6Th"><img src="https://i.imgur.com/rHoB6Th.png" width="700px" title="source: imgur.com"  caption="고양이"/></a>

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

임의의 $6 \times 6$ 픽셀의 grayscale 이미지가 있다고 하자. 해당 이미지는 픽셀의 밝기에 따라 값을 갖는 $6\times 6$ 행렬로 표현이 가능해.

밝을수록 더 큰 값을 갖고 어두울수록 작은 값을 갖게 된다고 할 때, 밑에 왼쪽의 이미지를 행렬로 표현하면 오른쪽과 같아.

<a href="https://i.imgur.com/rHoB6Th"><img src="https://i.imgur.com/rHoB6Th.png" width="700px" title="source: imgur.com" /></a>

위에 표시된 부분이 세로 윤곽선이고 세로 윤곽선 위치에서는 **좌/우 밝기가 불연속** 하다는 특징을 가지고 있지.

이런 특징을 갖는 영역을 찾기 위해 우리는 다음의 **$3 \times 3$ 행렬**과 **합성곱 연산**을 정의해보자. 

<a href="https://i.imgur.com/rHoB6Th"><img src="https://i.imgur.com/rHoB6Th.png" width="700px" title="source: imgur.com" /></a>

정확하게 정의하면, 이미지 $A\ (n \times n)$ 와 행렬 $F\ (f \times f)$의 합성곱 연산으로 나온 결과 행렬 $B$는 다음과 같이 정의된다.

$B_{i,j} =\ sum\ (A_{[i, i+f-1], [j, j+f-1]} * F)$, 여기에서 $*$는 elementwise product를 의미해.

합성곱 연산에서 사용되는 행렬 $F$는 **filter**라고 불리며, 앞으로 우리는 이 용어를 자주 사용할꺼야.


위에 그림과 같이 1열을 모두 1, 2열을 모두 0, 3열을 모두 -1 인 filter를 사용하는 경우, filter와 합성곱 연산되는 영역의 좌/우 밝기에 따라 다음의 결과를 가져.
 
 - 좌/우 밝기가 비슷한 경우: 0의 값을 가짐
 - 좌/우 밝기가 다른 경우(왼쪽이 더 밝은 경우): 양수 값을 가짐
 - 좌/우 밝기가 다른 경우(왼쪽이 더 어두운 경우): 음수 값을 가짐

세로 윤곽선 영역의 경우, 좌/우 밝기가 불연속적으로 변화하기 때문에 합성곱 연산의 결과의 절대값이 커진다.

다시 말하면, 합성곱 연산의 결과인 행렬 $B$에서 검은색 혹은 흰색의 세로 영역이 원래 이미지의 세로 윤곽선 영역이 돼.

위의 예제에서 이미지 $A$의 중간에 세로 윤곽선이 있고, 필터 $F$와 합성곱 연산을 한 결과인 $B$의 중간 영역에 음수 값을 갖는 검은 세로 영역으로 검출.




