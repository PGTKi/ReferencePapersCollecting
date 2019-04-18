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
  
# Preliminary of CNN
- I<sup>(n)</sup>: the n-th input feature map
- OK<sup>(n)</sup>: the m-th output feature map
- K<sup>(m,n)</sup>: kernel between n-th input and m-th output feature maps (all features map and kernels are two-dimensional, otherwise specified)
- I<sup>(n)</sup><sub>(r,c)</sub>: the neuron at location (r, c) of I<sup>(n)</sup>
- K<sup>(m,n)</sup><sub>(r,c)</sub>：the synapse value at position (i, j) of K<sup>(m,n)</sup>

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20CONV%20Operation.PNG)

The loops marked by the inner-box are unrolled to operate in parallel, and the operations in outer-box execute sequentially.

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Unrolling.PNG)

- three types of parallelism: 
    - Feature map Parallelism (FP): unrolled with factors < Tm, Tn >
    - Neuron Parallelism (NP): unrolled with factors < Tr, Tc >
    - Synapse Parallelism (SP): unrolled with factors < Ti, Tj >
    
# Baseline Architecture

The dataflow describes how the operands are dispatched, gathered, and updated among many distributed processing elements (PE).
    - SFSNMS: Systolic: synapse parallelism (SP)
    
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20unrolling%20SFSNMS.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20snapshot%20SFSNMS.PNG)
    
    - SFMNSS: 2D-Mapping: neuron parallelism (NP)
    
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20unrolling%20SFMNSS.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20snapshot%20SFMNSS.PNG)
        
    - MFSNSS: Tiling: feature map parallelism (FP)

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20unrolling%20MFSNSS.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20snapshot%20MFSNSS.PNG)

- Limitations
    - Fixed data direction
    - Fixed data type
    - Fixed data stride
# FlexFlow: Flexible DataFlow Architecture
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Architecture.PNG)
## PE 
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20PE.PNG)
## Complementary Parallelism
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20data%20mapping.PNG)
## DataFlow
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Dataflow.PNG)
### DataFlow1: Distribution Layer to Local Store
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Local%20Store%20Access.PNG)
### DataFlow2: Local Store to Operator
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20FSM.PNG)
### DataFlow3: Neuron and Kernel Buffers to Distribution Lay
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Kernel%20Buffer%20Data%20Placement%20and%20Data%20Transmission%20Pattern.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Neuron%20Buffer%20Data%20Placement%20and%20Data%20Transmission%20Pattern.PNG)
## Determining Parallelism
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Parallelism%20constraints.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20PE%20utilization.PNG)

    
    
