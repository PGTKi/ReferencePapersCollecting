----
## Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks ##
----
*Yu-Hsin Chen  ; Tushar Krishna ; Joel S. Emer ; Vivienne Sze*

[[论文]](https://ieeexplore.ieee.org/document/7738524/)
# Abstract
- **Motivation**
  - significant data movement from on-chip and off-chip
- **Methods**
  - exploiting data reuse in a multilevel memory hierarchy
  > the hardware needs to be reconfig￾urable to support different shapes
  - exploiting data statistics 
    - compression and data adaptive processing 
- **Proposal:Eyeriss**
  - A spatial architecture
    - 168 processing elements (PEs)
    - four-level memory hierarchy
      - PE scratch pads (spads)
      - inter-PE communication(168 PEs: 12 × 14)
      - on-chip global buffer (GLB): 108-kB
      - off-chip DRAM: through a 64-b bidirectional data bus
  - A CNN dataflow, called Row Stationary (RS)
    - partition: feature map tesors(input & output), kernel t
  - A network-on-chip (NoC) architecture
    - both multicast and point-to-point single-cycle data delivery to support the RS dataflow.
  - Run-length compression (RLC) and PE data gating
    - exploit the statistics of zero data in CNNs 
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss2Eyeriss%20system%20architecture.PNG)
# CNN BASICS
![]()
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss1Equation%20Computation%20of%20a%20CNN%20layer.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss1Computation%20of%20a%20CNN%20layer.PNG)
# SYSTEM ARCHITECTURE
## top-level control coordinates:
  - 1)traffic between the off-chip DRAM and the GLB through the asynchronous interface
  - 2)traffic between the GLB and the PE array through the NoC
  - 3)operation of the RLC CODEC and ReLU module
## processing procedure
> **loads the configuration bits into a 1794 b scan chain**
   - *configure the accelerator: mappings; NoC data delivery patterns*
>> **loads tiles of the ifmaps and filters from DRAM for processing**
>>> **writte back to DRAM**
# ENERGY-EFFICIENT FEATURES
## Row Stationary：reducing data movement
> Data accesses to the high-cost DRAM and GLB are minimized through maximally reusing data from the low-cost spads and inter-PE communication.
- 1-D Convolution Primitive in a PE
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss1-D%20convolution%20primitive%20in%20a%20PE.PNG)
  - each PE can use the local spads for both convolutional data reuse and psum accumulation
  - spads capacity: 1) S for a row of filter weights; 2) S for a sliding window of ifmap values; 3) 1 for the psum accumulation
- 2-D Convolution PE Set
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss2-D%20convolution.PNG)
1) shares the same row of filter or ifmap across primitives 
2) accumulates the psums from multiple primitives together
  - height: filter rows (R);  width: ofmap rows (E)
 - 

## RLC: exploiting data statistics

# Qusetion
1. strip mining
> The strip-mined PE set width is determined by a process that optimizes for overall energy efficiency as introduced in [[32]](https://ieeexplore.ieee.org/document/7551407).
2. Eyeriss
