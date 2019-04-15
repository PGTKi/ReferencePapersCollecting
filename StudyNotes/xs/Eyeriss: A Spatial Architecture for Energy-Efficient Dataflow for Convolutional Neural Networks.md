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
