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
category: system
author: hongsun Jang
description: Markdown summary with different options
---

# Deepspeed nvme offload 사용시 flow 정리

# 파일명 규칙 정리

- swap_out_gradient -> 
```
{self.param_id}_gradient_{offset}_{length}.tensor.swp
```



## Gradient의 흐름
backward 계산 후 allreduce_gradients에서 fp32형태의 gradient가 ssd로 swap_out된다. 
이 중 주의할 것은 swap_out이 될때 모두 swap out 될 수 있는 것은 아니다. unswapped가 남아있다.
optimizer step전에 gradient 가 optimizer state와 함께 swap_int된다.





## 가장 중요한 _swap_out_gradients() -> zero/stage3.py
- 유일하게 partition_grads함수에서 호출된다. 

- __reduce_and_partition_ipg_grads에서 호출된다.

    1. reduce_independent_p_g_buckets_and_remove_grads
        - reduce_ready_partitions_and_remove_grads
            backward의 hook으로 걸려있다.
        - create_reduce_and_remove_grad_hooks()

        - param_tmp.grad_fn.next_functions[0][0]에 register hook을 통해 grad_fn 한개가 계산될때마다 call된다.

    2. independent_gradient_partition_epilogue: backward 후 항상 1번 called
        - overlapping_partition_gradients_reduce_epilogue

        - self.optimizer.overlapping_partition_gradients_reduce_epilogue -> runtime/engine.py에서 호출된다
    
        - allreduce_gradients




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
        - 만약 has_been_updated가 아니면 free_param 함수를 수행한다
            - 만약 ds_tensor의 final_location이 NVME면, nvme_swapper의 <code>remove_partition_and_release_buffer</code>를 실행한다.
        - 만약 has_been_update인 상태이면?
    2. line 831에 _convert_to_deepspeed_param 함수가 정의되어 있다.
    2. <code>Init</code> 함수가 정의되어 있다.



## 의문점들
1. param_coordinator.hierarchy?

3. param_coordinator.__inflight_param_registry
