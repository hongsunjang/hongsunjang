---
title: "별로지만 어쩔수없이 써야하는 conda 정리(miniconda)"
layout: post
date: 2023-07-14 22:44
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

# Miniconda vs anaconda
사실 Miniconda로도 충분하다 여러번 환경설정을 해야하는 경우가 많고, 특히 local에서 사용할 것이라면 오히려 miniconda가 더 편리하다.

# <code> conda create </code>
```
conda create -n <env_name>
```

클론 옵션은 hard/soft link로 enviornment를 생성해서 빠르게 복사가 되는 장점이 있지만, 만약 소스에 직접 수정을 가해야하는 경우 절대 쓰면 안된다. 한 가상환경에서의 수정이 다른 가상환경까지 영향을 미친다.
```
conda create -n <env_name> --clone <target_env_name>
```

똑같지만 다른 환경을 만들고 싶다면
```
conda create -n <env_name> --clone <target_env_name> --copy
```
을 추가하면 hard/soft link를 피할 수 있다.

# 분산 딥러닝 환경 구축
```
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.6 -c pytorch -c conda-forge
```
