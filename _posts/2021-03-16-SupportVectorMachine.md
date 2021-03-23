---
title: "[SVM-01] Support Vector Machine 개요"
category: Machine Learning
tag: ["Support vector machine"]
---

오늘은 1990년대에 Vladimir Vapnik 가 제안한 Support vector machine 방법의 개요를 simple 하게 살펴볼께요.

Patrick Winston 교수님의 MIT 6.034 Artificial Intelligence(Fall 2010) 강의를 참고했어요. (내용을 노트처럼 쭉 정리한거라, 두서가 없을 수 있어요..)

"공간을 어떻게 두 개로 나눌 수 있을까요? 즉, 결정 경계(Decision Boundary)를 어떻게 만들 수 있을까요?"

신경망이나, K-nearest neighborhood 클러스터링, 트리는 이런 목적에 사용되는 방법들이죠.

위의 질문에 대한 하나의 해답이 바로 support vector machine 이에요. 

오늘은 이 질문에 대한 Vapnik 교수님의 직관적이고 간단한 해답을 찾아가는 과정을 공유헤볼께요.

---

## Widest Street Approach

Positive/Negative 예제가 있는 이런 공간을 생각해볼까요. 

<a href="https://i.imgur.com/IOUkS5H"><img src="https://i.imgur.com/IOUkS5H.png" width="300px" title="source: imgur.com" /></a>

Nearest neighborhood / tree / Neural network 를 사용하면 아래와 같이 boundary를 그을 수 있어요.

<table>
  <tr>
    <td>Nearest neighborhood</td>
     <td>Tree</td>
     <td>Neural network</td>
  </tr>
  <tr>
    <td><img src="https://i.imgur.com/iqEJ6T3.png" width="300px" title="source: imgur.com"></td>
    <td><img src="https://i.imgur.com/GmMx2Fp.png" width="300px" title="source: imgur.com"></td>
    <td><img src="https://i.imgur.com/kQpk0Cg.png" width="300px" title="source: imgur.com"></td>
  </tr>
 </table>
 

Vapnik 교수님은 문제를 새로운 방식으로 접근했어요. 일단 문제를 Positive/Negative 예제를 나누기 위해 어떤 직선을 그을 것인가 라는 것으로 제한했어요.

나눌 수 있는 다양한 직선을 그을 수 있지만, 그 중 어떤 것을 그어야할까요?

위의 질문은 기준에 대한 이야기가 없으므로, 조금 더 명확히 표현하는 것이 좋아보입니다.

질문을 조금 바꿔서, 우리가 그은 직선이 새로운 예제가 들어와도 문제가 없으려면 어떻게 그어야할까요?

다음 문제를 생각해볼까요.

```
Q. Positive/Negative 예제는 각각 일정 반경 r 로 공을 던질 수 있다고 합시다. 우리는 두 예제를 가로지르는 도로를 만들려고 해요. 
도로를 어떻게 만드냐에 따라 각 예제에서 던진 공이 도로의 중앙선을 넘을수도 있고 아닐수도 있겠죠.

우리는 여러 가능한 도로 중에서 던진 공이 중앙선을 넘지 않으면서 r 을 가장 크게 할 수 있는 도로를 찾고 싶습니다. 어떻게 하면 될까요?
```

위 문제의 해답이 곧 두 예제를 가장 잘 구분하는 직선을 찾는 것과 동일하며, 조금만 생각해보면 두 예제 사이에 가장 넓은 도로를 만드는 것이 해답이 됩니다.

<table>
  <tr>
    <td><img src="https://i.imgur.com/DEQnd8e.png" width="300px"></td>
    <td><img src="https://i.imgur.com/phOxumi.png" width="300px"></td>
  </tr>
 </table>

위의 두 그림을 보면, 가장 넓은 길을 찾는 경우 다른 길보다 더 큰 반경 $r$ 로 공을 던질 수 있죠.

Vapnik 교수님은 두 예제 사이에 가장 넓은 길(widest street)을 찾아서 그 중앙선을 boundary 로 사용하는 아이디어를 제안했어요.

이 방법을 _Widest street approach_ 라고 부르죠.

그럼 가장 넓은 길을 어떻게 찾을 수 있을까요?

## Widest street 을 찾는 방법

그럼 실제로 positive/negative 예제가 주어져있을 때 widest street 을 어떻게 찾을 수 있을까요?

공간에 어떤 길이 있다면, 그 길은 어떤 부등식으로 표현 가능할꺼에요. 실제로 모든 길은 다음 부등식으로 표현할 수 있어요.

 - $-1 < w^\intercal x + b < 1$, $w,b$ 는 길에 대응되는 unique 한 value.

<a href="https://i.imgur.com/DEQnd8e"><img src="https://i.imgur.com/nBHH3rd.png" width="300px" title="source: imgur.com" /></a>_@ 임의의 street_

Positive/Negative 를 가로지르는 여러 도로들은 각각 대응되는 $w, b$ 에 의해 $-1 < w^\intercal x + b < 1$ 으로 표현 됩니다.

그럼 부등식을 활용해서, positive/negative 예제 사이에서 가장 넓은 길(widest street)을 찾아볼까요? 이는 다음을 만족하는 $w, b$ 를 찾는 것과 같아요.

$\min_{(w,b)} \|\|w\|\|$\\
subject to $w^\intercal x_+ + b \geq 1$ & $w^\intercal x_- + b \leq -1$, $x_+, x_-$ 는 각각 positive, negative 예제


