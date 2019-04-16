-----
Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow　for Convolutional Neural Networks
-----
*Yu-Hsin Chen ; Joel Emer ; Vivienne Sze*

[[文章]](https://ieeexplore.ieee.org/document/7551407)
# Data Handling
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss1Equation%20Computation%20of%20a%20CNN%20layer.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/raw/master/StudyNotes/xs/pictures/Eyeriss1Computation%20of%20a%20CNN%20layer.PNG)
- convolutional reuse: Each filter weight is reused E\*E times in the same ifmap plane, and each ifmap pixel, i.e., activation, is usually reused R\*R timesin the same filter plane.
- filter reuse: Each filter weight is further reused across the batch of N ifmaps in both CONV and FC layers.
- ifmap reuse: Each ifmap pixel is further reused across M filters (to generate the M output channels) in both CONV and FC layers.

# EXISTING CNN DATAFLOWS
  A. Weight Stationary (WS) Dataflow:
  Once a weight is fetched from DRAM to the RF of a PE, the PE runs through all NE<sup>2</sup> operations that use the same filter weight.
  
  B. Output Stationary (OS) Dataflow: 
  
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss-%20OS%20dataflow%20variants.PNG)
  
  - SOC-MOP: processing a single plane of ofmap at a time.  **convolutional reuse** & **psum accumulation**
  - MOC-MOP: processing multiple ofmap planes with multiple pixels in the same plane at a time. **convolutional reuse** & **ifmap reuse**
  - MOC-SOP: processing multiple ofmap channels but with only one pixel in a channel at a time. **ifmap reuse**
  
  C. No Local Reuse (NLR) Dataflow:
    (1) it does not exploit data reuse at the RF level
    (2)it uses inter-PE communication for ifmap reuse and psum accumulation.

  PEs within the same group: read the same ifmap pixel but with different filter weights from the same input channel. 
  
  Different PE groups read ifmap pixels and filter weights from different input channels. 
  
  The generated psums are accumulated across PE groups in the whole array.
    
# ROW STATIONARY
- Primitive
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss1-D%20convolution%20primitive%20in%20a%20PE.PNG)

- Two-Step Primitive Mapping
  - Logical Mapping:
  
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss2-D%20convolution.PNG)
  
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss-Shape%20Parameter.PNG)
  
  N × M ×C logical PE sets required: ifmap/filter channels (C), number of filters (M) and fmap batch size (N)
  
  - Physical Mapping(folding): 
    - Folding multiple logical PEs from the same position of different sets onto a single physical PE exploits input data reuse and psum accumulation at the RF level.
    - Different processing passes run sequentially on the entire physical PE array.
- Support for Different Layer Types
  - FC Layer: same as CONV layers
  - POOL Layer: swapping the MAC computation with a MAX comparison function
  
# Energy Efficiency Analysis
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss-%20ENERGY%20COST%20Relative.PNG)

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss-%20reuse%20ifmps%20filter.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss-%20Input%20Data%20Access%20Energy%20Cost.PNG)

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss-%20psum%20accumulation.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss-%20Psum%20Accumulation%20Energy%20Cost.PNG)
  
  

## Storage requirement 
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20MAPPING%20PARAMETERS.PNG)

- spads:
  - 1) p ×q × S for the rows of filter weights from q channels of p filters
  - 2) q × S for q sliding windows of ifmap values from q different channels
  - 3) p for the accumulation of psums in p ofmap channels
- GLB: 
  - n×q×r 2-D ifmaps 
  - n×m 2-D psums 

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Eyeriss%20PARAMETERS%20OF%20AlexNet.PNG)


