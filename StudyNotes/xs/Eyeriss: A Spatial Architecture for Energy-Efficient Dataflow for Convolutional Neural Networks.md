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
  - SOC-MOP: processing a single plane of ofmap at a time.  **convolutional reuse** & **psum accumulation**
  - MOC-MOP: processing multiple ofmap planes with multiple pixels in the same plane at a time. **convolutional reuse** & **ifmap reuse**
  - MOC-SOP: processing multiple ofmap channels but with only one pixel in a channel at a time. **ifmap reuse**
  
  C. No Local Reuse (NLR) Dataflow:
    (1) it does not exploit data reuse at the RF level
    (2)it uses inter-PE communication for ifmap reuse and psum accumulation.

