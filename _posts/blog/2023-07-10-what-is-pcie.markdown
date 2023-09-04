---
title: "What is PCIe?"
layout: post
date: 2023-07-10 22:44
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

# PCIe bifuration 이란?

bifurcation 의 의미는 "the division of something into two branches or parts" 라고 정의되어 있다. 
정의에 따라 예를 들어 PCIe x8 card slot이 two x4 chunks로 나누어 꼳힐 수 있다는 것이다. 


# Memory-Mapped I/O Base (MMIOBase)
컴퓨터 시스템에서 I/O 디바이스들은 그들의 제어 동작을 호스트와 통신할 수 있는 register 를  가지고 있다. 하지만 MMIO의 경우 이런 device의 register들이 host의 메모리 address space에 연결된다. 따라서 호스트에서 I/O device에 대한 동작제어를 regular load/store instruction으로 수행할 수 있다. 

MMIOBase는 MMIO region의 base address를 의미한다. 이것은 보통 datasheet나 documentation에 명시되어 있다. 

# PCIe는 4가 요소 이루어진다
RAM에 Mapping 되어있다는 것은 allocated into RAM을 의미하지 않는다. 
actual data가 저장된 곳은 PCI device이다. 
만약 device가 1MB의 memory-mapped space를 요청했다면, BIOS는 address 0x1000/0000 to 0x1010/0000을 할당해준다. 
이건 physical RAM 용량을 계속 차지하고 있을 것이라는 의미가 아니다. 
만약 이 주소 space에 데이터를 write하게 되면 이는 PCI device에 해당하는 memory space로 자동으로 전달되는 것이다. 


1. configuration space: 4KB  정도의 address space로 system memory를 차지하지만, 이는 실제 device의 register에 저장되어 있다. 여기에는 vender ID, device ID등 다양한 device에 대한 정보도 얻을 수 있다.

# What is BAR (base address register )
Memory-mapeed


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