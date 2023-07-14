---
title: "Deepspeed NVMe baseline 분석"
layout: post
date: 2023-07-14 22:44
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

# Deepspeed nvme offload 사용시 flow 정리
## DeepSpeedZeRoOffload 클래스 초기화
- <code> convert_to_zero_paramters </code>함수를 통해 partition_parameters.py 파일의 <code>Init</code> 객체가 만들어진다.


## self._pre_step();

<code> deepspeed/runtime/zero/stage3.py</code>

- self.paramter_offload._get_param_coordinator().hierarchy = 0

    이게 무슨뜻이냐.. 일단 적혀있는 설명은 Finished Tracing at Beginning of Step

- self.parameter_offload = initialize_ds_offload()
     // DeepSpeedZeRoOffload 객체 초기화

<code> deepspeed/runtime/zero/parameter_offload.py</code>
- DeepSpeedZeRoOffload가 정의되어 있다.
- PartitionedParameterCoordinator() 
    // PartitionedParamterCoordinator 객체 초기화

<code> deepspeed/runtime/zero/partitioned_param_coordinator.py </code>

- PartitionedParameterCoordinator가 정의되어 있다.

## self._partition_all_parameter()

- <code> deepspeed/runtime/zero/stage3.py</code>
    1. self.parameter_offload.partition_all_parameters()

- <code> deepspeed/runtime/zero/paramter_offload.py</code>
    1. partition_all_parameter 함수가 정의되어 있다.
    2. param_coordinator에 대해 <code>release_and_reset_all</code> 를 실행한다.
    3. 그리고 모든 paramter들의 state가 NOT_AVAILABLE인지 확인한다.

- <code> deepspeed/runtime/zero/partitioned_param_coordinate.py</code>
    1. release_and_reset_all 함수가 정의되어 있다.
        - 모든  paramter들에 대해 <code> inflight_param_registry </code>에 포함되는지 확인
        - 모든 paramter들에 대해 <code>ds_active_sub_modules.clear()</code> 수행, ds_active_sub_modules는 set 자료형으로 모든 요소를 지우는 python 함수이다.
        - 모든 parameter들에 대해 <code>__release_param</code> 수행

    2. __release_param이 정의되어 있다.
        - 해당 torch.nn.Parameter에 대해 <code>partition</code> 함수를 실행한다.

- <code> deepspeed/runtime/zero/partition_parameters.py</code>
    1. ds_active_sub_modules는 set 자료형
    2. line 947에 torch.nn.Paramter에 대해 <code>partition</code> 함수가 정의되어 있다.
        - partition 함수는 partition_param 함수를 부른다
    3. partition_param 함수가 정의되어 있다.
        - param.ds_status 가 AVAILABLE일때 offload를 수행한다. 수행후 NOT_AVAILABLE으로 바꿔준다.
        - 
        - 
    2. line 831에 _convert_to_deepspeed_param 함수가 정의되어 있다.
    2. <code>Init</code> 함수가 정의되어 있다.



## 의문점들
1. param_coordinator.hierarchy?

3. param_coordinator.__inflight_param_registry