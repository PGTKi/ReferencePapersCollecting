-----
FlexFlow: A Flexible Dataflow Accelerator Architecture for Convolutional Neural Networks
-----
*Wenyan Lu ; Guihai Yan ; Jiajun Li ; Shijun Gong ; Yinhe Han ; Xiaowei Li*

[[文章]](https://ieeexplore.ieee.org/abstract/document/7920855/)
# Introduction
- motivation
    a big mismatch between the parallel types supported by computing engine and the dominant parallel types of CNN workloads
- fine-grained parallelism
  - feature map (FP): Systolic
  - neuron parallelism (NP): 2D-Mapping
  - feature map parallelism (FP): Tiling
- contributions:
  - complementary parallelism: multiple types of parallelism
  - dataflow optimization from on-chip buffers to PEs: RS, RA, IADP, IPDR
  
