---
title: "깔끔하게 정리하는 NVIDIA 개발 환경설정(Drivers, CUDA, CUDNN)"
layout: post
date: 2023-07-04 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- markdown
- elements
star: true
category: system
author: hongsun Jang
description: Markdown summary with different options
---

# Nvida driver 설치
먼저 Nvidia driver가 이미 설치 되어 있는지 확인해야한다.
```
dpkg -l | grep -i nvidia
```
dpkg는 Debian 기반의 linux 시스템에서 사용되는 패키지 관리 도구이다.
패키지 설치를 위해 <code>.deb </code>확장자를 가진 패키지 파일을 사용할 수 있다.
Debian은 Linux 운영 체제의 하나로 여러가지 소프트웨어 패키지를 포함하고 있는 배포판이다. 
특히 Ubuntu는 Debian을 기반으로 더 개발된 배포판으로 따라서 Debian 의 패키지 관리 시스템을 공유하는 것이다.

만약 이미 설치되어 있다면 제거해 주고 다시 설치해야한다.
```
sudo apt remove -y --purge nvidia-*
```

<code>apt remove</code> 의 purge 옵션은 패키지와 관련된 모든 설정 파일과 데이터까지 완전히 제거한다. 

```
sudo apt autoremove -y
```

<code>apt autoremove</code> 는 nvidia 관련 패키지와 연관되었지만 남아있는 잔여 패키지를 제거해준다.


```
ubuntu-drivers devices
```
로 확인 후 적절한 nvidia driver를 설치하면 된다.
항상 <code>-open</code> <code>-server</code> 등 다양한 옵션이 있는데, <code>-open</code>의 경우 Linux 시스템에 커널에 결합되는 특성이 있고, <code>-server</code>는 드라이버를 독립적으로 관리하고 업데이트할 수 있는 서버 기반의 솔루션이다.

결론적으로 별로 크게 상관없지만, 리눅스 커널에 직접 결합되면 문제가 생기는 경우가 많기 때문에 서버기반의 솔루션을 추천한다.

설치 후 재부팅이 꼭 필요하다.


# CUDA 설치
https://developer.nvidia.com/cuda-toolkit-archive
에서 다운로드 받을 수 있다.

runfile로 설치하는 것을 권장한다. 주의할 점은 이미 nvidia driver는 설치했기 때문에, 다른 설치 옵션들은 체크 해제하고 cudatoolkit만 다운로드 받아야한다. 

```
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```
까지 <code>.bashrc</code> 나 <code>.profile</code> 에 원하는 방법으로 추가하면 완료.

보통 nvidia-smi에서 출력되는 드라이버에서 지원하는 가장 최신의 CUDA를 설치하면 conda 환경에서 하위 cudatoolkit들은 모두 지원이 되지만
분산 딥러닝 환경에서는 보통 설치할 pytorch 버전에 맞게 설치하는게 좋다.
왜냐하면 global cuda와 conda 환경으로 설치한 cudatoolkit이 버전이 일치하지 않으면 오류가 발생하는 경우가 많기 때문이다 (e.g. NVIDA/APEX)

따라서 에러가 발생하지 않고, 가장 자주 사용하는 환경은 다음과 같다.
CUDA Toolkit 11.6 Update 2
```
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.6 -c pytorch -c conda-forge
```

분산 딥러닝은 매우 불안정하기 때문에, 상위 pytorch 환경이 필요한 경우는 거의 없을 것이다...

```
nvcc --version
```
위 명령어로 cuda가 잘 설치되었는지 확인할 수 있다.


# CUDNN 설치
https://developer.nvidia.com/rdp/cudnn-download#a-collapse765-90 
에서 NVIDIA 회원가입 후 다운로드 받을 수 있다. 
Tar 파일을 다운로드 받은 후 
```
cd cuda
cp include/cudnn* /usr/local/cuda/include/
cp lib/libcudnn* /usr/local/cuda/lib64/
chmod a+r /usr/local/cuda/lib64/libcudnn* 
```
위 명령어로 설치할 수 있다.


```
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2 
```
위 명령어로 cudnn이 잘 설치 되었는지 확인할 수 있다.  






# NVIDIA APEX 설치

https://github.com/NVIDIA/apex
에서 설치 방법을 참조하자.

APEX 설치하기 전에 꼭 필요한 패키지가 조금 있다.
```
conda install -c conda-forge packaging
```
<code>conda-forge</code>의 경우 conda 패키지 관리자의 공식 채널 중 하나이다. 커뮤니티를 기반으로한 오픈소스이므로 엄격한 품질관리가 지원된다. <code>conda</code> 명령어의 default channel은 <code>defaults</code> 로 Anaconda, Inc 에서 제공한다. 따라서 필요하다면 conda-forge를 기본 channel로 설치해도 무방하다.

```
git clone https://github.com/NVIDIA/apex
cd apex
# if pip >= 23.1 (ref: https://pip.pypa.io/en/stable/news/#v23-1) which supports multiple `--config-settings` with the same key... 
pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-settings "--build-option=--cpp_ext" --config-settings "--build-option=--cuda_ext" ./
```






<!-- 
# Model 

![Screenshot from 2022-02-17 16-04-19.png](/assets/images/2021-02-17/Screenshot_from_2022-02-17_16-04-19.png)

- GPT **Generative Pre-trained Transformer**
    - Transformer base의 최초 논문
    - 이전 단어를 바탕으로 다음 단어를 예측하는 방법으로 훈련된다.
    - 문맥파악에 약점이 있다.
    
- BERT ****Bidirectional Encoder Representations from Transformers****
    
    : Nest Sentence prediction을 바탕으로 자연스럽게 문맥을 파악할 수 있다.
    
    - Masked 된 단어를 예측하는 양방향 (문맥) 바탕 예측으로 훈련된다.
    - NSP를 도입하여 문장간의 관계도 학습할 수 있다.

- Bert 기반 모델
    - RoBERTa(19년) - FaceBook ****A Robustly Optimized BERT Pretraining Approach****
        
        → 단방향 학습인 GPT가 Parameter 수와 학습데이터를 늘리자 좋은 성능? 왜?
        
        1. Bert의 고정된 Mask로 반복 학습 → Dynamic Masking 도입
        2. 두 문장간의 예측이 의미가 있을까? → NSP 제거
        3. 학습데이터 16G → 학습데이터 160G
        
    - ****ALBERT(19년) A L****ittle **BERT** for Self-supervised Learning of Language Representations
        
        NSP → SOP(Sentence order prediction)으로 학습
        
        Transformer 구조 개선 → 모델사이즈 감소
        
    - ELECTRA(20년)
        
        ****Pre-training Text Encoders as Discriminators Rather Than Generators****
        
        MLM(Masked Language Model)을 개선시킨 RTD(Replaced Token Detection)
        
        - 이해를 위한 이미지
            
            ![Screenshot from 2022-02-17 16-23-27.png](/assets/images/2021-02-17/Screenshot_from_2022-02-17_16-23-27.png)
            
        
        [MASK] 토큰을 작은 Generator로 생성하고 Discriminator로 가짜 토큰인지 아닌지 판별하는 방식으로 학습한다.
        
        효과 ? BERT는 [MASK] 토큰에 대해서만 학습하지만 ELECTRA는 모든 토큰에 대해서 학습할 수 있어 효율적인 학습이 된다.
        

![Screenshot from 2022-02-17 16-30-44.png](/assets/images/2021-02-17/Screenshot_from_2022-02-17_16-30-44.png)

![Screenshot from 2022-02-17 16-31-08.png](/assets/images/2021-02-17/Screenshot_from_2022-02-17_16-31-08.png)

- Pretrained Model의 목표
    1. Model downsizing: 모델의 크기가 너무 커서 메모리에 들어가지 못한다.
    2. Train Resource downsizing: 학습이 오래걸린다.
    3. Memory degradation: 모델에 따라 일정 수준이상 복잡해지면 모델 성능이 떨어진다
-->
