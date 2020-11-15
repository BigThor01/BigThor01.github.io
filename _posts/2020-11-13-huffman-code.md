---
title: 허프만(Huffman) 코드
category: Information Theory
tag: Huffman code
---

**허프만(Huffman) 코드**는 데이터를 효율적으로 압축하거나 전송할 때 사용하는 코딩 방법이야.

오늘은 허프만 코드가 어떻게 데이터를 압축할 수 있는지 그 원리에 대해 이야기해보자.

## 이진코드(Binary code)

이진코드는 모든 기호를 0 또는 1로 이루어진 이진 숫자 조합으로 나타내는 코딩 방법이야.

우리가 쓰는 디지털 기기에서 모든 기호는 0 또는 1로 이루어진 bit의 조합으로 구성되어있어.

예를들어 A, B, C, D 네 개의 문자는 다음과 같이 2 bit의 이진코드로 표현이 가능해.

 - A $\rightarrow$ 00
 - B $\rightarrow$ 01
 - C $\rightarrow$ 10
 - D $\rightarrow$ 11

우리가 알고 있는 아스키(ASCII) 코드는 128개 기호를 8 bit의 이진수로 표현한 대표적인 이진코드 중 하나야.

## 고정 길이 코드(Fixed length code)

고정 길이 코드(Fixed length code)는 모든 기호를 일정한 길이의 bit로 나타낸 코드야. 위에서 말한 아스키 코드는 모든 기호를 8 bit로 표현한 고정 길이 코드의 한 예시야.

만약 $k$개의 기호를 고정 길이 코드로 표현하기 위해서는 다음과 같이 $\log_2 k$개의 bit가 필요하지.

$2^{bit\ 수} = k\ \rightarrow\ {bit\ 수} = \log_2 k.$

부호화(encoding)된 기호를 다시 복호화(decoding)할 때, 고정 길이 코드의 경우 정해진 bit 수로 잘라서 처리하면 되는 간단함을 가지고 있지.

## 가변 길이 코드(Variable length coode)

고정 길이 코드는 다루기 쉽다는 장점이 있지만, 저장 공간 활용에 단점이 있다.

가변 길이 코드(Variable length code)는 기호에 따라 다른 길이의 bit로 표현하는 방법으로 저장 공간의 절약이라는 목적에서 사용된다.

가변 길이 코드는 입력된 문장에 포함된 문자의 빈도 수에 따라 다른 bit 수로 표현하게 된다.

예를들어, "ABAABCAAD"라는 문장이 있다고 하자. 4개의 문자를 고정 길이 코드로 나타내면 

 - 고정 길이 코드: A $\rightarrow$ 00,  B $\rightarrow$ 01, C $\rightarrow$ 10, D $\rightarrow$ 11
 
가 되고, 위의 문장은 "000100000110110011"의 18 bit로 표현된다.

이제 해당 문자의 빈도수에 따라 문자의 빈도수가 클수록 작은 bit로 표현해보도록 하자.

- 가변 길이 코드:  A $\rightarrow$ 0,  B $\rightarrow$ 10, C $\rightarrow$ 110, D $\rightarrow$ 111

그럼 위에 문장은 "010001011000111"의 15 bit로 표현이 돼. 빈도가 큰 문자를 작은 bit로 코드화하면서 문장의 총 bit 수를 줄이는 거지. 

## 접두어 코드(Prefix code)

가변 길이 코드 중에서도 접두어 코드(Prefix code)는 어떤 기호의 부호화된 결과도 다른 기호의 부호어의 접두어가 되지 않는 코드이다. 예를들어

 -  A $\rightarrow$ 0,  B $\rightarrow$ 01, C $\rightarrow$ 10, D $\rightarrow$ 011

은 A(0)은 B(01), D(011)의 접두어이기 때문에 접두어 코드가 아니다.

위의 경우 만약 내가 받은 부호가 "010"인 경우, 우리는 AC 또는 BA로 해석될 수 있어. 이런 경우를 구분하기 위해 접두어 코드가 아닌 경우 구분자를 필요로 해.


반면 접두어 코드는 어떤 기호도 다른 기호의 접두어가 되지 않기 때문에 이런 구분자가 필요가 없어. 접두어 코드의 한 예시는 다음과 같아.

- A $\rightarrow$ 0,  B $\rightarrow$ 10, C $\rightarrow$ 110, D $\rightarrow$ 111


## 허프만 코드(Huffman code)

그럼 어떻게 효율적으로 압축할 수 있는 가변 길이 코드를 만들 수 있을까? 허프만 코드는 대표적인 접두어 코드 중 하나야. 

특정 문자열 "ABCAABCDDEAADAEB"이 있을 때 허프만 코드로 만드는 방법을 알고리즘으로 설명해볼께.

  1. 각 문자의 발생 빈도를 작성한다.
  2. 순서대로 배열하고 가장 작은 두 개의 문자를 트리로 연결한다. 상위 노드에 빈도수의 합을 작성한다.
  3. 2에서 만들어진 빈도수를 포함해서 2를 반복한다.
  4. 트리가 완성되면, 상위 노드에서 왼쪽은 0, 오른쪽은 1을 부여한다.
  

<a href="https://i.imgur.com/mCd1XJG"><img src="https://i.imgur.com/mCd1XJG.png" width="700px" title="source: imgur.com" /></a>
