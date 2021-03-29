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
