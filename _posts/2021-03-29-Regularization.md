---
title: "ML 기초 - Regularization"
category: Machine Learning
tag: ["Regularization"]
---

모형 학습시에 overfitting 문제는 항상 관심이 되는 이슈에요.

Overfitting 완화/방지를 위해 training example 확보, feature selection, dropout, early stopping, regularization 등의 방법을 사용해요.

오늘은 그 중에서 tuning parameter 를 사용해서 그 정도를 flexible/tunable 하게 조절할 수 있는 _regularization_ 에 대해 이야기해볼까요?

해당 내용은 다음의 자료를 참고해서 정리했어요.

 - [Alexander Ihler 교수님의 Youtube 강의](https://youtu.be/sO4ZirJh9ds)
 - [CS194-10 Lecture4 강의노트](https://people.eecs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2004.pdf)
 - [Why should non convexity be a problem in optimization](https://scicomp.stackexchange.com/questions/23948/why-should-non-convexity-be-a-problem-in-optimization)
 - [Constraint and penalty term are equivalent](https://math.stackexchange.com/questions/335306/why-are-additional-constraint-and-penalty-term-equivalent-in-ridge-regression)

---
## Model flexibility 와 overfitting 의 관계

모델 flexibility 는 overfitting 과 밀접한 관련이 있어요.

모델이 flexible 할수록 training data 에 대한 reconstruction error 는 점점 줄어들게 됩니다. 반대로 모델이 덜 flexible 할수록 training data 에 대한 error 는 증가하게 되죠.

10개의 training example 에 대한 polynomial regression 을 생각해볼까요. 밑에 데이터는 $y = 2 \sin (1.5x) + \epsilon$ 으로 생성하였고 파란선은 underlying true model 이에요.

코드로 구한 비교 그림

Polynomial order 가 증가할수록 training data 에 대한 error 는 감소하는 것을 알 수 있어요. 

하지만 이는 마냥 좋은 것은 아닙니다. 바로 overfitting 이 일어났기 때문이에요. 모델이 flexible 해지면 underlying true model 에 가까워지지만 어느 순간부터 data 의 random error 까지 학습하는거죠.

그럼 우리는 다음 질문을 할 수 있어요.

 - **Overfitting 을 방지하기 위한 적절한 model flexibility 는 어떻게 결정할까?**

여기에 대한 답을 주는 것이 regularization 이에요. 자세히 알아봅시다.

## Coefficient magnitude 와 flexibility

위의 예시에서 order 를 10으로 하는 경우, 어떤 10개 data 가 들어와도 error 가 0인 매우 flexible 한 모형이 됩니다.

얘를 덜 flexible 하게 만들려면 어떻게 해야할까요? 여러 방법 중 한가지 방법은 **coefficient 의 범위를 0 근처의 영역으로 묶어두는 방법**이 있어요.

Data가 어떻게 들어오든, coefficient 가 움직이는 범위는 0 근처의 제한된 영역이므로 coefficient 변동은 줄어들게
되죠. 심지어 어떤 coefficient 는 거의 0 이 되어서 모델에 영향을 주지 않게 되기도 하구요.

결과적으로 모델은 덜 flexible 해지는 거죠. 이것이 바로 regularization 의 아이디어 입니다.

 - **Data 에 fit 시키되, coefficient magnitude 를 제한해서 덜 flexible 하게 만든다.**
 
코드로 구한 비교 그림

## $L_p$ regularization

그럼 coefficient 를 어떻게 0 근처로 당겨올(shrink) 수 있을까요?

한가지 방법은 coefficient 를 0 주위의 일정 거리 내의 영역으로 제한하는 방법이 있어요.

즉, 기존의 목적함수에 다음 제약조건을 추가하는거죠. 

 - $\min_\beta reconstruction\ error$, sub to $\|\|\beta\|\|_p <= t$ ... 식(1)

여기에서  $\|\|\beta\|\|_p$ 는 $L_p$ norm 으로, 식(1)을 이용해서 solution 을 구하는 방식을 $L_p$ regularization 이라 불러요.

$p$ 에 따라 모양은 다르겠지만 모두 0 주위의 영역으로 coefficient 를 제한하는 역할을 하며, $t$ 에 따라 shrink 정도가 달라지죠. ($L_p$ norm 은 뒤에서 간략히 설명하겠습니다.)

식(1)은 Lagrange multiplier 를 이용해서 다음 식으로 바꿀 수 있어요.

 - $\min_\beta reconstruction\ error\ +\ \lambda \|\|\beta\|\|_p$ ... 식(2)

식(2)에서 $\lambda$ 는 tuning parameter, $\lambda \|\|\beta\|\|_p$ 는 penalty term 이라 불러요.

따라서 우리가 찾고자 하는 solution 은 다음 식을 풀어서 나오게 됩니다.

 - $\min_\beta reconstruction\ error\ +\ penalty$ ... 식(3)

## $\lambda$ 에 따라서 shrink 정도가 결정됩니다.

식(3)에서 알 수 있듯이 목적함수는 reconstruction error 와 penalty 의 합으로 표현됩니다.

Penalty term 은 data 와 무관하게, 0 에서 멀어질수록 커지게 되어요. $\lambda$ 는 penalty 가 반영되는 정도를 결정하기 때문에 그 값에 따라 다음 효과를 가져요.

 - $\lambda$ 증가 : data의 영향력은 줄고, $\beta$는 0에 가까워짐 $rightarrow$ data에 따른 variability 감소, bias 증가.
 - $\lambda$ 감소 : data의 영향력은 늘고, $\beta$는 OLS에 가까워짐 $rightarrow$ data에 따른 variability 증가, bias 감소.
 
그림 : lambda에 따른 차이 그림

---
## (참고) $L_p$ norm 이 어떻게 생겼을까?

$L_p$ norm 은 어떻게 생겼는지 추가적으로 알아볼까요?

## (참고) Maximum a Posteriori(MAP) 와 regulariazation 과의 관계

