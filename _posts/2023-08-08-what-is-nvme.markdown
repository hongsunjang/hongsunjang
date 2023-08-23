---
title: "What is NVMe?"
layout: post
date: 2023-08-08 22:44
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

# What is PCIe?
Peripheral Component Interconnect Express
주변 장치 / 요소 간 / 상호 연결 (고속 버전) 

하나의 Link에서는 보내거나 받는 것 중 하나의 역할만 할 수 있어
Simplex 2개가 합쳐진 Dual-simplex connection


Serial computer expansion bus standard

# Why Serial?

병렬 버스는 signal을 sender에서 reciever로 보내느데 걸린 시간이 중요하다. 이를 Flight time
flight time은 clock의 주기보다 적어야하는데 이와 같은 상황이 어려워진다.

전송 측과 수신 측에서 clock 도착시간이 다르다. clock skew?

정해진 clock 에서 signal이 도착하는 시간이 모두 다른건 signal skew

-> clock은 data stream에 포함되어 signal과 같이 도착한다. -> flight time은 의미 없다.
-> signal skew는 한 레인에서 한 bit만 들어온다.

128b/130b encoding을 사용해서 128bit를 보내기 위해 추가적으로 보내는 비트가 2bit여서 총 130bit씩 보낸다는 뜻이다.

# PCIe 구성요소
- Root Complex
- Switch
- Bridge
- PCI endpoint device
    - Legacy PCI endpoint

# Enumeration : PCIe topology를 발견하고 bus number와 시스템 자원을 할당하는 것


다른 버스와의 인터페이스를 제공한다.
# PCIe configuration space
버스에 삽입된 장비를 자동으로 찾는 방법

BDF(Bus, device Function) 는 PCie slot에 따라 부여되고 이를 스캔하면 디바딩스 정보 알 수 있다.
따라서 드라이버나 diagnostic software는 해당 디바이스의 configuration spacae에 접근할 수 있어야한다.

PCI configureation space는 용량이 매우 적기 때문에, 사용할 공간을 따로 잡아 디바이스와 OS가 어디를 사용할지 공유해야하는데 이를 Base Address Register에 내용을 입력한다. 

Memory-mapped I/O 스페이스 외에도 버스위의 각각 device function은 




#

PCIe has numerous imporvement s

single Lane -> two lanes for each direction

PCI host and all devices share a comon set of address, data, control lines

point-to-point topology 

with separate serial links connecting every device to the root complex

하지만 이 shared bus topology때문에 


# I/O virtualizaiton



# SSD의 구조

NAND 플래시 메모리 셀

SSD는 플래시 메모리를 기반으로한 저장 매체이다.

비트들은 Floating-Gate transistor로 구성된 셀에 저장된다. 

Floating-gate 트렌지스터에 전압이 가해지면 셀의 비트가 쓰여지거나 읽혀지게 된다.

NOR, NAND가 있는데 NAND플래시 메모리 밖에 안들어볼 것이다.
왜 NAND냐? NAND gate가 가격이 싸다.
NAND는 write가 느린 특성이 있다.


문제는 NAND gate는 수명이 제한적이다.
트렌지스터는 셀에 전자를 저장하는데 매번 P/E (Program & Erase)를 하게되면 일부 전자가 트랜지스터에 갇힌다.
전자가 일정수준을 넘어가면 그 셀을 사용 불가능하다.

SLC,MLC, TLC

1. P/E cycle: 100k/10k/5k
2. bits per cell: 1/2/3
3. Read latency : 25/50/100
4. Write latnecy: 250/900/1500
5. Erase latency: 1500/3000/5000

RAM, L2 cache
read latency: 0.1 / 0.001
write latency: 0.1 / 0.001

SLC타입의 SSD는 제조비용이 크기때문에
대부분 MLc/TLC 타입이다. 
쓰기 빈도가 높다면 SLC 타입이 최적이다.
읽기가 매우 높다면 TLC  타입이 적절하다.

# 주요 컴포턴ㄴ트
일반적인 인터페이스는 PCIe이다.
 
실제 데이터가 저장되는 공간은 Flash memory package로 구성된다.
