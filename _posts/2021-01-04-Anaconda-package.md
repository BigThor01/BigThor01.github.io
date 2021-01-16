---
title: Pytorch, tensorflow 설치 (GPU 사용)
category: Etc.
tag: ["Pytorch", "Tensorflow","GPU","CUDA","cuDNN"]
---

오늘은 딥러닝 분석을 위해 python 모듈인 **pytorch** 와 **tensorflow** 를 설치해볼께.

내 PC 에는 GPU 가 있어서, GPU 를 사용할 수 있는 환경에서 setting 을 진행할꺼야.

사실 setting 하는 방법은 다른 블로그나 영상에서도 자세히 나와있지만, 내가 진행한 작업에 대한 기록을 남기려고 해.

---
## GPU 확인하기

일단 PC 에 있는 어떤 GPU 가 있는지 확인을 해보았어. 다음과 같이 _Window + R_ 을 눌러서 _dxdiag_ 를 치면 다음의 창이 뜨고 디스플레이 탭을 보면 내 GPU 가 _NVIDIA GeForce GTX 1650 with Max-Q Design_ 임을 알 수 있어.

<a href="https://i.imgur.com/VLXmorQ"><img src="https://i.imgur.com/VLXmorQ.png" width="600px" title="source: imgur.com"/></a>
_@Window + R → dxdiag_

<a href="https://i.imgur.com/cxz148g"><img src="https://i.imgur.com/cxz148g.png" width="600px" title="source: imgur.com"/></a>
_@DirectX 진단 도구 > 디스플레이_


## 가상 환경 만들기

처음으로는 일단 가상 환경을 만들어보자. 

왜 가상환경에서 작업을 하는가? 내가 알고있는 이유는, 모듈을 설치하거나 버전을 바꾸거나 할 때 여러 오류가 발생할 수 있는 가능성이 있어.

그래서 가상으로 환경을 만들어서 거기에서 모듈을 설치해서 쓰는거지. 만약 문제가 생기면 그냥 가상환경을 지워버리면 되는 장점이 있어.

_Anaconda Prompt_ 를 실행하고 **conda create --name temp_1 python python=3.6** 명령어를 실행하면 python 버전 3.6 의 temp_1 이라는 가상 환경이 만들어져. 

<a href="https://i.imgur.com/m2xqWqp"><img src="https://i.imgur.com/m2xqWqp.png" width="600px" title="source: imgur.com"/></a>
_@가상 환경 만들기_

**activation temp_1** 으로 만든 가상 환경을 실행할 수 있고, 일단 setting 전에 **pip install jupyter notebook** 로 _jupyter notebook_ 을 모듈을 설치했어. 

<a href="https://i.imgur.com/x3rdyLJ"><img src="https://i.imgur.com/x3rdyLJ.png" width="600px" title="source: imgur.com"/></a>
_@가상 환경 실행 및 jupyter notebook 모듈 설치_

## CUDA Tookit/cuDNN 설치하기 (tensorflow, pytorch 에서 GPU 활용 위한 setting)

일단 내 PC 에 NVIDIA 그래픽 드라이버가 설치되어 있는지 확인해보자.

_명령프롬프트(cmd)_ 에서 **nvidia-smi** 명령어를 실행해서 다음과 같이 뜨면 제대로 NVIDIA 그래픽 드라이버가 설치되어있는 것이고, 빨간색으로 표시한 버전이 내 그래픽카드가 지원하는 최대 CUDA 버전이야. 내 드라이버는 CUDA 11.0 까지 지원하고 있어.

<a href="https://i.imgur.com/gxxVVkD"><img src="https://i.imgur.com/gxxVVkD.png" width="600px" title="source: imgur.com"/></a>
_@그래픽 드라이버 확인 및 지원하는 CUDA 버전 확인_

나는 [_CUDA Toolkit 10.1 update2_](https://developer.nvidia.com/cuda-toolkit-archive) 버전을 설치하고 다운받은 exe 파일을 _사용자 정의 설치_ 로 설치했어.

그 다음 [_cuDNN v7.6.5 for CUDA 10.1_](https://developer.nvidia.com/rdp/cudnn-archive) 버전을 설치했어. 

처음에는 _cuDNN v8.0.3_ 을 설치했는데 _tensorflow-gpu == 2.1.0_ 를 구동할 때, **Could not load dynamic library 'cudnn64_7.dll', dlerror: cudnn64_7.dll not found** 에러가 발생하였는데 이게 cuDNN 버전 문제여서 _cuDNN v7.6.5_ 을 설치했더니 잘 해결되었어.

<a href="https://i.imgur.com/WMYRnIP"><img src="https://i.imgur.com/WMYRnIP.png" width="600px" title="source: imgur.com"/></a>
_@cuDNN v7.6.5 for CUDA 10.1 다운로드_

아까 _CUDA Toolkit_ 을 설치하면 _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1_ 폴더에 생겨. 여기에 _cuDNN_ 의 압축을 풀어서 생긴 폴더/파일을 복사해서 _v10.1_ 폴더 밑에 덮어쓰기를 진행해.

<a href="https://i.imgur.com/2c5cYRv"><img src="https://i.imgur.com/2c5cYRv.png" width="600px" title="source: imgur.com"/></a>
_@Toolkit/cuDNN 압축 해제 및 설치_

이제 다 설치되었으니 시스템 환경 변수에 _CUDA\_PATH, CUDA\_PATH\_V10\_1_ 변수에 **C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1** 를 추가를 해야해. 

이제 cmd 창에서 **nvcc --version** 을 입력하면 설치된 CUDA 버전을 확인할 수 있고, 여기에 빨간 줄처럼 release 10.1 이 써있으면 잘 설치된거야.

<a href="https://i.imgur.com/D0GX9Qz"><img src="https://i.imgur.com/D0GX9Qz.png" width="600px" title="source: imgur.com"/></a>
_@CUDA Toolkit 설치 확인_


## Tensorflow 설치하기

이제 아까의 _Anaconda Prompt_ 에서 temp_1 가상환경이 활성화된 상태에서 **pip install tensorflow-gpu==2.1.0** 명령어로 2.1.0 버전을 설치했어.

<a href="https://i.imgur.com/zR5unfN"><img src="https://i.imgur.com/zR5unfN.png" width="600px" title="source: imgur.com"/></a>
_@tensorflow-gpu==2.1.0 버전 설치_

아래와 같이 ipython 을 실행하면 _Anaconda Prompt_ 에서 python 코드를 실행할 수 있어. **import tensorflow as tf** 를 실행했을 때, **Successfully opened dynamic library cudart64_101.dll** 이 나오면 잘 설치된거야. 

파이썬 인터프리터에서 tensorflow 가 GPU 를 사용할 수 있는지는 **tf.config.list_physical_devicex('GPU')** 코드를 실행했을 때, 다음과 깉이 나오면 문제 없이 GPU 를 사용할 수 있는거야.

<a href="https://i.imgur.com/gOPkHAB"><img src="https://i.imgur.com/gOPkHAB.png" width="600px" title="source: imgur.com"/></a>
_@tensorflow가 GPU를 사용할 수 있는지 확인_


## Pytorch 설치하기

Pytorch 를 설치를 위해서는 [_pytorch 홈페이지_](https://pytorch.org/get-started/locally/) 에서 나의 환경을 입력하여 명령어를 확인할 수 있어.

<a href="https://i.imgur.com/TEZkUMF"><img src="https://i.imgur.com/TEZkUMF.png" width="600px" title="source: imgur.com"/></a>
_@Pytorch 홈페이지_

내 환경에 맞는 명령어는 **pip install torch==1.7.1+cu101 torchvision==0.8.2+cu101 torchaudio===0.7.2 -f https://download.pytorch.org/whl/torch_stable.html** 이고, _Anaconda Prompt_ 에서 실행하면 pytorch 를 설치할 수 있어.

<a href="https://i.imgur.com/dOPyCzk"><img src="https://i.imgur.com/dOPyCzk.png" width="600px" title="source: imgur.com"/></a>
_@Pytorch 설치_

Pytorch 가 잘 설치되었는지, GPU 사용이 가능한지 확인하기 위해 ipython 에서 다음의 코드로 확인할 수 있어.

<a href="https://i.imgur.com/kPW6kTK"><img src="https://i.imgur.com/kPW6kTK.png" width="600px" title="source: imgur.com"/></a>
_@GPU 사용가능 여부 및 torch 버전 확인_

**torch.cuda.get_device_name(0)** 로 GPU 확인이 가능하고, **torch.cuda.is_available()** 을 통해 GPU 사용이 가능한지 확인할 수 있어.

내 pytorch 버전은 **torch.__version__** 으로 1.7.1 임을 확인할 수 있지.

그럼 pytorch, tensorflow 에서 GPU 활용을 위한 setting 은 다 되었고, 앞으로 사용할 일만 남았어.
