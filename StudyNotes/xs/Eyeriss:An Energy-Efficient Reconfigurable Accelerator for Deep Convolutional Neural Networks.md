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
      - inter-PE communication
      - on-chip global buffer (GLB)
      - off-chip DRAM
  - A CNN dataflow, called Row Stationary (RS)
    - partition: feature map tesors(input & output), kernel t
  - A network-on-chip (NoC) architecture
    - both multicast and point-to-point single-cycle data delivery to support the RS dataflow.
  - Run-length compression (RLC) and PE data gating
    - exploit the statistics of zero data in CNNs 
# CNN BASICS
![inference](https://github.com/PGTKi/ReferencePapersCollecting/blo
