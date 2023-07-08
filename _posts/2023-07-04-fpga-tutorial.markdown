---
title: "Xilinx FPGA tutorial"
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

# FPGA 튜토리얼 정리이다.
Feature tutorial과 Design tutorial로 나눠져 있다. 

Feature tutorial은 Specific features or flows of Vitis를 배운다

Design tutorials은 higher-level concepts or design flows, walk through specific examples or reference designs and more and complex designs or applications

Feature tutorial은 크게 얻을 수 있는게 많이 없고, Vitis를 잘 사용하는 법을 얻을 수 있었다.

이번 포스트에서는 Design tutorial을 중점으로 진행한다.

먼저 software 구현 (CPU)로 구현한 성능 측정 결과는 다음과 같다.
![sw_run](../assets/images/2023-07-05/sw_run.png)

먼저 hardware 구현 (FPGA)로 구현한 성능 측정 결과는 다음과 같다.


먼저 Task에 대해 이해해볼 것이다. 바로 2D video convolution filter로 
```
Video Resolution = 1920 x 1080
Frame Rate (FPS) = 60
Pixel Depth (Bits) = 8
Color Channels(YUV) = 3
Minimum Throughput = 1920 * 1080 * 60 * 8 * 3 = 378MB/s
```

최소한 378MB/s의 출력량이 필요하다는 것을 알 수 있다.
하지만 이는 SW(CPU)결과에 비하면 요구량이 초과되는 것을 확인할 수 있다.

## Hardware implementation
Convolution 연산의 특징에 대해 더 생각해보아야한다.
four-level nested loop이고, output pixel 결과값을 기준으로 병렬적으로 수행할 수 있다.

filter size가 15x15이기 때문에 inner two loop의 경우 dot product 연산이고, 
하나의 output pixel 당 225 MAC operations이 필요하다.

MACs per Cyle = 1 이라고 가정할때
Hardware Fmax(MHz) = 300
Throughput = freq. / #mac ops  = 300/225 = 1.33 (MPixels/s) = 1.33MB/s

따라서 우리는 이 1.33MB/s 를 가지고 병렬화를 하면된다.

첫번째 optimization은 loop unrolling이다. loop unrolling을 통해 연산을 동시에 수행할 수 있다.
따라서 두개의 inner loop에 unrolling을 적용하면 15*15=225배의 속도향상을 얻을 수 있다.

하지만 memory bandwidth를 확인해보면
```
Input memory bandwidth = Fmax * Input pixels read per output pixel = 300 * 225 = 67.5 GB/s
```
하지만 실제로 확인해보면 convolution의 input은 서로 다른 pixel이 아니라 입력이 중복되기 때문에 실제로 그렇지는 않다.


# Bloom filter

Document filter은 굉장히 많이 쓰인다. 실제로 이를 계하기 위해서 document relevence를 계산하기 위한 방법에 대해 알아볼 것이다.




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