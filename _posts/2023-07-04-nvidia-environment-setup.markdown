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
category: blog
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