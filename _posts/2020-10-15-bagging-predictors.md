---
title: 배깅(Bagging)
category: Machine Learning
tag: Bagging
---


오늘은 **배깅(Bagging)**에 대해 이야기해볼까해. 

이번 포스트는 Bagging predictors(Leo Breiman,1996), 서울대 김용대 교수님의 강의자료 및 StackExchange를 참고해서 작성했어.

## Predictor가 unstable한 경우, prediction은 부정확해

Machine learning은 learning data를 이용해서 rule을 만드는 방법이야.

우리가 $L = \\{(\mathbf{x}_n, y_n); n =1, ..., N\\}$라는 leanring set을 가지고 있고, $\varphi$ 방법을 이용해서 예측 모형을 만든다고 생각해보자. 

만들어진 predictor는 $\varphi(x, L)$로 표기할 수 있지. 여기서 predictor는 $L$에 의해 결정되기 때문에 $L$이 인자로 들어있어.

만약 predictor $\varphi(x, L)$가 unstable하다고 가정해볼까? 즉, $L$의 변화에 따른 predictor의 변동이 크다고 생각해보자.

그럼 prediction을 할 때 어떤 문제가 생길까? 

### regression case

regression case에서 prediction error는 보통 MSE를 통해 측정하게 돼.  MSE를 풀어서 쓰면 우리는 다음의 식을 구할 수 있어.

$ MSE = E_{y,L} (y - \varphi (x,L))^2 = E_{y,L} (y - E(y\|x) + E(y\|x) - \varphi (x,L))^2$

$ = Var (y\|x) + (E(y\|x) - E_L (\varphi(x,L))^2 + Var_L (\varphi (x,L))$

$ = something + Var_L (\varphi(x,L))$


위의 식에서 알 수 있듯이 MSE는 $\varphi$의 variance에 의해 표현되고, $\varphi$가 unstable할수록 prediction error는 커지게 돼.

### classification case

binary classification case를 생각해보면, prediction error는 오분류율을 가지고 측정하게 돼. 오분류율은 다음의 식처럼 구할 수 있어.

$ Error = E_{y,L} 1(y \neq \varphi (x,L)) = P_y,L (y \neq \varphi (x,L)$

$ =  P (y = 1) P(\varphi(x,L) = 0 ) + P(y =0) P(\varphi (x,L) =1 ) = (1-2p) P(\varphi(x,L) = 1 ) + p$

,where $p = P(y=1)$


$\varphi$가 order-correct($argmax_j P(Y = j) = argmax_j P(\varphi(x,L) = j$) 하다고 가정해보자. 보통 $\varphi$는 order-correct의 속성을 갖도록 만들어지므로 이렇게 가정하는거는 문제가 없어.

위의 식을 그래프로 표현하여 살펴보면 $\varphi$의 variance가 클수록 prediction error는 커지게 돼. 





[어떤 $\varphi$가 unstable할까?](https://stat.snu.ac.kr/ydkim/courses/2017-1/addm/Chap7.1-Bagging.pdf)




 

$E_L (y - \varphi(x,L))^2 = y^2 - 2y E_L \varphi(x,L) + E_L \varphi^2 (x,L)$
$\geq  y^2 - 2y E_L \varphi(x,L) + E_L \varphi (x,L)^2  = (y - E_L \varphi(x,L))^2$

## 


우리 [decision tree](http://www.yes24.com/Product/Goods/78569687)를 생각해보자. Decision tree는 learner이고i, 
우리는 y를 잘 예측하는 것을 보통 목적으로 varphi를 만들어. 

많은 learning 방법이 있지만 오늘은 decision tree를 예로 들어 설명할까해.

데이터에 fitting을 통해서 예측하게 된다.

variance + bias로 구분돼 (regression 문제에서)

variance-bias tradoff 라는 개념이 있어. 즉 어떤 방법이 bias가 커지는 만큼 variance는 작아짐 (데이터의 특성을 잘 반영하는데 오차까지 반영하냐..)

그래서 우리는 good-fit을 찾기 위해서. validation을 사용해서 적절한 tree의 size를 구하게 돼.

위와 같이 하나의 single learning set을 이용해 학습을 하는 경우, 해당 데이터 set에 전적으로 의존해서 learner를 만들기 때문에 Single learner.. 즉 overfit을 하지 않으면서 

우리는 한 가지 궁금증을 갖게 돼. bias와 variance를 모두 작게 가져가는 방법은 없을까?

Single learning set을 사용해서 fitting을 한다. 우리는 complexity를 높여서 training에 굉장히 잘맞는 모델을 만들 수 있다. 단 이러한 경우. variance가 증가.

cross validation, 해당 방법으로 추정한 경우에 variance와 bias의 최적점을 찾는다. 음음.
그래서 validation을 줘서, MSE를 줄이는 가장 best한 모델을 사용한다. 추정치가 됨.Just one case. L이 given된 경우. 약간의 bias가 있을 수 있음?
bias.bias.bias. L의 variance에 의해서 발생하는 사실은 이것보다 

근데 우리가 궁금한 것은 다음이다. bias와 variance를 모두 줄이는 방법은 없을까?

overfitting. underfitting. good-fit.



## 배깅이란?



## 배깅은 어떻게 작동할까?

한 가지 예제를 생각해볼까해. 우리에게 multiple learning set이 있다고 가정해보자.

${L_k: k = 1,....,K}$. 각각에서 $\varphi_k (x) = \varphi (x,L_K)$를 만들 수 있겠지? 

우리가 알 수 있는 것은 뭘까? 바로 $\varphi_A (x) = E_L (\varphi (x, L_k)$를 만들 수 있어. 즉, $L$에 의해 발생하는 $\varphi(x,L)$의 분산을 없앨 수 있는거지.

우리는 다음을 구할 수 있어.

$(y - \varphi_A (x))^2  = (y - E_L \varphi (x, L))^2 \leq E_L (y - \varphi (x,L))^2.$

위의 말은 뭐냐하면, 임의의 L을 가지고 만든 MSE보다 작다.

$Var(\varphi(x,L))$만큼 크다. 

왜 이렇게 될까?

$E_y,L (y - \varphi (x, L))^2  = Var(y|x) + E_L [E(y|x)-\varphi(x,L)]^2 = Bias^2 + Var_L (\varphi(x,L).$

후자의 part가 없어진다. 즉, variance가 없어진다. 



## 임베딩이 중요한 이유

임베딩에는 말뭉치(corpus)의 의미, 문법 정보가 응축돼 있습니다. 임베딩은 벡터이기 때문에 사칙연산이 가능하며, 단어/문서 관련도(relevance) 역시 계산할 수 있습니다. 

최근 들어 임베딩이 중요해진 이유는 따로 있습니다. 바로 전이 학습(transfer learning) 때문입니다. 전이 학습이란 특정 문제를 풀기 위해 학습한 모델을 다른 문제를 푸는 데 재사용하는 기법을 의미합니다. 예컨대 대규모 말뭉치를 미리 학습(pretrain)한 임베딩을 문서 분류 모델의 입력값으로 쓰고, 해당 임베딩을 포함한 모델 전체를 문서 분류 과제를 잘할 수 있도록 업데이트(fine-tuning)하는 방식이 바로 그것입니다. 물론 전이 학습은 문서 분류 이외의 다양한 다른 과제에도 적용할 수 있습니다. 

전이 학습 혹은 프리트레인-파인 튜닝 메커니즘은 사람의 학습과 비슷한 점이 있습니다. 사람은 무언가를 배울 때 제로 베이스에서 시작하지 않습니다. 사람이 새로운 사실을 빠르게 이해할 수 있는 이유는 그가 이해를 하는 데에 평생 쌓아 온 지식을 동원하기 때문입니다. 자연어 처리 모델 역시 제로에서 시작하지 않습니다. 우선 대규모 말뭉치를 학습시켜 임베딩을 미리 만들어 놓습니다**(프리트레인)**. 이 임베딩에는 의미, 문법 정보가 녹아 있습니다. 이후 임베딩을 포함한 모델 전체를 문서 분류 과제에 맞게 업데이트합니다**(파인 튜닝)**. 이로써 전이 학습 모델은 제로부터 학습한 모델보다 문서 분류 과제를 빠르게 잘 수행할 수 있습니다. 

다음은 [네이버 영화 리뷰 말뭉치(NSMC)](https://github.com/e9t/nsmc)를 가지고 영화 리뷰 문서의 극성(polarity)을 예측하는 모델의 정확도(accuracy)와 학습 손실(training loss)를 그래프로 나타낸 것입니다. `FastText`는 이 모델의 입력값을 단어 임베딩 기법의 일종인 FastText를 사용한 것이고, `Random`은 랜덤 임베딩을 썼다는 뜻입니다. 후자는 다시 말해 학습을 제로에서부터 시작했다는 뜻입니다. 그래프를 보면 아시겠지만 임베딩 품질이 좋으면 수행하려는 태스크(극성 분류)의 성능이 올라갑니다. 아울러 모델의 수렴(converge) 역시 빨라집니다.



<a href="https://imgur.com/u8yHOqM"><img src="https://i.imgur.com/u8yHOqM.png" title="source: imgur.com" width="500px" /></a>

<a href="https://imgur.com/5R661Va"><img src="https://i.imgur.com/5R661Va.png" width="500px" title="source: imgur.com" /></a>



품질 좋은 임베딩은 잘 담근 김치와 같습니다. 김치 맛이 좋으면 물만 부어 끓인 김치찌개 맛도 좋습니다. 임베딩 품질이 좋으면 단순한 모델로도 원하는 성능을 낼 수 있습니다. 모델 구조가 동일하다면 그 성능은 높고 수렴(converge)은 빠릅니다. 자연어 처리 모델을 만들고 서비스할 때 중요한 구성 요소 하나만 꼽으라고 한다면, 저는 주저하지 않고 ‘임베딩’을 꼽을 것입니다. ELMo(Embeddings from Language Models), BERT(Bidirectional Encoder Representations from Transformer), GPT(Generative Pre-Training) 등 자연어 처리 분야에서 당대 최고 성능을 내는 기법들이 모두 전이 학습 혹은 프리트레인-파인 튜닝 메커니즘을 사용하는 것은 우연의 일치가 아닙니다.





## 이 책이 다루는 범위

[한국어 임베딩](http://www.yes24.com/Product/Goods/78569687)에서는 NPLM(Neural Probabilistic Language Model), Word2Vec, FastText, 잠재 의미 분석(LSA), GloVe, Swivel 등 6가지 단어 수준 임베딩 기법, LSA, Doc2Vec, 잠재 디리클레 할당(LDA), ELMo, BERT 등 5가지 문장 수준 임베딩 기법을 소개합니다. 이외에도 다양한 임베딩 기법이 있지만 두 가지 원칙에 입각해 일부만 골랐습니다. 우선 성능이 안정적이고 뛰어나 현업에 바로 적용해봄직한 기법을 선택했습니다. 또 임베딩 기법의 발전 양상을 이해하는 데 중요한 역할을 하는 모델을 포함했습니다. ‘정보의 홍수’ 속에서 살아가는 독자들에게 핵심에 해당하는 지식만을 전해주고 싶었기 때문입니다. 기타 임베딩 기법들은 대부분, 이 책에서 소개하는 11개 모델의 변형에 해당하기 때문에 독자 여러분이 추가로 공부하고 싶은 최신 기법이 있다면 이 책에서 가지를 쳐 나가는 식으로 학습하면 수월할 거라 생각합니다. 

이와 관련해 XLNet이라는 기법을 짚고 넘어가야겠습니다. XLNet은 구글 연구팀이 2019년 상반기 발표한 모델로, 공개 당시 20개 자연어 처리 데이터셋에서 최고 성능을 기록해 주목받았습니다. 출간을 한 달 정도 늦춰 가며 목차와 내용을 전반적으로 손질할 수밖에 없었습니다. 그러나 직접 실험한 결과 XLNet의 파인 튜닝 성능(분류)이 BERT보다 뒤지는 것은 물론, 동일한 하이퍼파라미터(hyperparameter)로도 점수가 들쭉날쭉한 양상을 보였습니다. XLNet 저자 공식 리포지터리(https://github.com/zihangdai/xlnet)에도 비슷한 사례가 꾸준히 보고되고 있으나 출간 직전인 2019년 9월 현재까지 납득할 만한 해결책이 제시되지 않고 있습니다. 이에 아쉽기는 하지만 XLNet 관련 장을 1판에서는 제외하고 2판 이후를 기약하기로 했습니다. 그럼에도 XLNet을 공부하고 싶은 독자가 있다면 다음 링크를 확인하시면 됩니다.



- [XLNet](https://ratsgo.github.io/natural language processing/2019/09/11/xlnet/)





## 튜토리얼

[한국어 임베딩](http://www.yes24.com/Product/Goods/78569687)은 각 임베딩 기법의 이론적 배경을 설명함과 동시에 공개돼 있는 한국어 말뭉치로 실습하는 것을 목표로 합니다. 이 책의 모든 실습 코드는 다음 깃허브 리포지터리에 공개돼 있습니다. 코드는 예고 없이 수정될 수 있으니 다음 리포지터리의 최신 코드를 받아 실행하기를 권합니다.



● https://github.com/ratsgo/embedding



책에 소개된 코드(특히 bash 스크립트)를 그대로 실행하고 싶은데 일일이 타이핑하기엔 너무 길어 불편할 수 있습니다. 복사해서 붙여 넣기 쉽도록 다음 페이지에 기법별로 스크립트를 정리해 놓았습니다. 



● https://ratsgo.github.io/embedding





## 도서 구매 안내

전국 주요 서점과 온라인으로 구매할 수 있습니다. 온라인 서점 링크는 다음과 같습니다.



- [YES24](http://www.yes24.com/Product/Goods/78569687)
- [교보문고](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791161753508)
- [알라딘](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=206404643)
