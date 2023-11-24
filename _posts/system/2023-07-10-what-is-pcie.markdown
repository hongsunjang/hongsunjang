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
category: system
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
