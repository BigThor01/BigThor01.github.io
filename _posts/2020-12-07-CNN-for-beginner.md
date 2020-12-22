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

<a href="https://i.imgur.com/hSAZCZH"><img src="https://i.imgur.com/hSAZCZH.jpg" width="700px" title="source: imgur.com"/></a>
_@고양이_

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

즉, 어떤 뉴런은 수직 윤곽선, 다른 뉴런은 수평 윤곽선 ... 등의 low level 특성에서 반응한다는거야.

이런 low level 특성에 대한 뉴런의 반응이 연쇄적으로 전달되면서 high level 특성을 인지하게 되고 최종적으로 사물을 인식할 수 있어.

합성곱 신경망은 이런 인식 체계를 본따서 만들어졌어.

그럼 합성곱 신경망의 개요를 살펴볼까?

### Input / output



