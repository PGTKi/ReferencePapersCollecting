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
- O<sup>(m)</sup>: the m-th output feature map
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

K = 3

m = 0, n = 1, r = 3, c = 1

FIFOs whose size equals to the input feature map width minus the kernel width (here is 12−3=9). 
 
    - SFMNSS: 2D-Mapping: neuron parallelism (NP)
    
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20unrolling%20SFMNSS.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20snapshot%20SFMNSS.PNG)

Tr = 3, Tc = 3

m = 1, n = 1, r = 0, c = 0, i = 0, j = 1

At each clock cycle, Tr or Tc neurons are received and shifted from right to left or down to up across PEs, and one synapse is distributed to all PEs.

    - MFSNSS: Tiling: feature map parallelism (FP)

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20unrolling%20MFSNSS.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20snapshot%20MFSNSS.PNG)

Tm = 4, Tn = 2

m = 0, n = 0, r = 5, c = 2, i = 0, j = 2

- Limitations
    - Fixed data direction
        - 2D-Mapping: shifting input neurons right to left or down to up 
        - Systolic: shifting output neurons left to right
    - Fixed data type: The data in all PEs has same attribute
    - Fixed data stride: The operations for the operands (neu￾rons or synapses) of all PEs are carried out synchronously
# FlexFlow: Flexible DataFlow Architecture
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Architecture.PNG)
## PE 
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20PE.PNG)

Each PE row can complete one convolution and serve to one output neuron. 

For PEs in FlexFlow, operands are directly derived from on￾chip buffers through vertical and horizontal buses to each PE, and
buffered in randomly accessed local storages.

## Complementary Parallelism
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20data%20mapping.PNG)

Unrolling mode for C1: < Tm = 2, Tr = 1, Tc = 2, Tn = 1, Ti = 1, Tj = 4 >.

Unrolling mode for C1: < Tm = 2, Tr = 1, Tc = 2, Tn = 2, Ti = 1, Tj = 2 >.

## DataFlow
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Dataflow.PNG)
### DataFlow1: Distribution Layer to Local Store
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/A76FB57A-1405-49A7-8CD8-5FB8CFE649DF.jpeg)
- Relax Alignment (RA)

The neurons can be assigned to the two PE rows in alignment way, i.e. forwarding identical neurons to PEs within one column.

- Relax Synchronization (RS)

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20RS.PNG)

### DataFlow2: Local Store to Operator
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20FSM.PNG)



### DataFlow3: Neuron and Kernel Buffers to Distribution Lay
- In-Advanced Data Placement(IADP)

        arrange the data for each on-chip buffer in advance according to the logical grouping of PEs. 
      
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Kernel%20Buffer%20Data%20Placement%20and%20Data%20Transmission%20Pattern.PNG)

- In-Place Data Replication (IPDR)
        
        As shown in Figure 12(b), every data read in reading controller is replicated Tr ×Tc times.

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Neuron%20Buffer%20Data%20Placement%20and%20Data%20Transmission%20Pattern.PNG)

the factors < Tm, Tr, Tc > of current layer always equal to factors < Tn, Ti, Tj > of next layer

## Determining Parallelism
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20Parallelism%20constraints.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/FlexFlow%20PE%20Utilizations.PNG)

    
    
