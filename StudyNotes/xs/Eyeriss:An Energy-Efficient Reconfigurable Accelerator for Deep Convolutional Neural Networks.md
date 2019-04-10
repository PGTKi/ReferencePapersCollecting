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
      - 11 × 55 (CONV1), 5×27 (CONV2), and 3×13 (CONV3–CONV5).
    - More Than 168 PEs：strip mining(CONV1 is strip-mined to 11×7)
    - Width Larger Than 14：divided into separated segments that are mapped independently to the array(CONV2 is divided into 5 ×14 and 5 × 13)
    - Height Larger Than 12: not supported
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20Mapping%20of%20the%20PE%20sets.PNG)
- Dimensions Beyond 2-D in PE Array
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20dimensions%20beyond%202-D.PNG)
  - filter reuse(different ifmaps): [Fig. 6(a)]
  - Multiple 2-D Convolutions in a PE Set
    - ifmap reuse(different filters):[Fig. 6(b)]By interleaving
    - psum accumulation(different channels): [Fig. 6(c)]By interleaving
    
     e.g. Each PE runs p × q primitives simultaneously from q different channels of p different filters. 
    1) p ×q × S for the rows of filter weights from q channels of p filters; 
    2) q × S for q sliding windows of ifmap values from q different channels;
    3) p for the accumulation of psums in p ofmap channels.
  - Multiple PE Sets in the PE Array
  
  The PE array fits r × t PE sets in parallel that run r different channels of t different filters simultaneously. 
  CONV1 and CONV3 have t = 2 and 4. CONV4 and CONV5 have r = t = 2.
   
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20Mapping%20of%20the%20PE%20sets.PNG) 
  
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20MAPPING%20PARAMETERS.PNG)

  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20PARAMETERS%20OF%20AlexNet.PNG)
  
  - PE array processing passes
  
    In a pass, each input data are read only once from the GLB, and the psums are stored back to the GLB only once when the processing is finished.
  
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20Scheduling%20of%20processing%20passes.PNG)
  
  A pass is assumed to process three channels (q × r) and four filters (p × t).
## RLC: exploiting data statistics
**Consecutive zeros with a maximum run length of 31 are represented using a 5-b number as the Run. The next value is inserted directly as a 16-b Level, and the count for run starts again.**
1) reduce DRAM accesses using compression
2) skip the unnecessary computations to save processing power

  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20Encoding%20of%20the%20RLC.PNG)
  
# SYSTEM MODULES
## Global Buffer
- 108 kB:communicate with DRAM
  -100 kB: ifmaps and psums 
  - 8 kB (two banks of 512-b × 64-b SRAMs):filter weights to compensate for insufficient off-chip traffic bandwidth.
- 25 banks: each of which is a 512-b ×64-b (4 kB) SRAM. Each bank is assigned entirely to ifmaps or psums.
## Network-on-Chip
data delivery between the GLB and the PE array as well as between different PEs.
    - different convolution strides (U) [CONV1]
    - a set is divided into segments [CONV2]
    - multiple sets are mapped onto the array simultaneously[CONV4][CONV5]
- Global Input Network
a ***single-cycle* multicast** from the GLB to a group of PEs that receive the same filter weight, ifmap value, or psum.
12 MCs on the Y-bus; 14 MCs on each of the X-buses
5 b to support maximum 32 ifmap rows passing in diagonal
- GINs' data bus width
  - filter/psum GINs: 64 b (4b×16 b)
  - ifmap GINs: 16 b

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20Architecture%20of%20the%20GIN.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20ifmap.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20Mapping%20of%20the%20PE%20sets.PNG)

## Processing Element and Data Gating

The numbers of filters (p) and channels (q) that the PE processes at once are statically configured into the control of a PE.

- pipeline
  - one stage for spad access, and the remaining two for computation
- Spads
  - filter spad(SRAM): 224-b ×16-b 
  - ifmap and psum spads(registers): 12 b ×16 b and 24 b ×16 b, respectively
- Data gating logic
  - zero ifmap value detected, disable MAC -> 45% power consumption decrease
  
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20PE%20architecture.PNG)
# Qusetion 
1. strip mining
> The strip-mined PE set width is determined by a process that optimizes for overall energy efficiency as introduced in [[32]](https://ieeexplore.ieee.org/document/7551407).
2. Eyeriss
  - Is Eyeriss a widespread, common or widely accepted accelerator for DCNN?
3. Mapping parameters
> For a given CNN shape, these parameters are determined by an optimization process that takes: 1) the energy cost at each level of the memory hierarchy and 2) the hardware resources, including the GLB size, spad size, and number of PEs, into account [[32]](https://ieeexplore.ieee.org/document/7551407).
