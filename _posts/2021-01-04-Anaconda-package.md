---
title: Pytorch, tensorflow 설치 (netstat)
category: Etc.
tag: ["Pytorch", "Tensorflow"]
---

오늘은 딥러닝 분석을 위해 python 모듈인 **pytorch** 와 **tensorflow** 를 설치해볼께.

사실 설치하는 방법은 정말 많은 블로그나 영상으로도 확인할 수 있어. 그래도 이렇게 작성하는 것은 내가 진행한 작업에 대한 기록의 의미야.

기본적으로 anaconda3 이 설치된 상황에서 진행해볼께. 내 python version 은 3.8.3 이야.

일단 모듈 설치를 하기 전에, 가상 환경을 하나 만들고 거기에다가 설치를 해보자.

왜 가상환경에서 작업을 하는가? 내가 알고있는 이유는, 모듈을 설치하거나 version을 바꾸거나 할 때 여러 오류가 발생할 수 있는 가능성이 있어.

그래서 가상으로 환경을 만들어서 거기에서 모듈을 설치해서 쓰는거지. 만약 문제가 생기면 그냥 가상환경을 지워버리면 되는 장점이 있어.

<a href="https://i.imgur.com/NLLS1C0"><img src="https://i.imgur.com/NLLS1C0.png" width="800px" title="source: imgur.com" /></a>

나는 "temp_210104" 라는 이름으로 가상환경을 만들었어. 

<a href="https://i.imgur.com/CLvWjvn"><img src="https://i.imgur.com/CLvWjvn.png" width="800px" title="source: imgur.com" /></a>

위와 같이 activate 명령어를 이용해서 가상환경을 활성화할 수 있어.

<a href="https://i.imgur.com/2NO8CVz"><img src="https://i.imgur.com/2NO8CVz.png" width="800px" title="source: imgur.com" /></a>

<a href="https://i.imgur.com/Ssh6Lah"><img src="https://i.imgur.com/Ssh6Lah.png" width="800px" title="source: imgur.com" /></a>

tensorflow, keras 는 위처럼 _pip install 패키지명_ 으로 설치해야해.

