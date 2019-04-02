----
**HyPar: Towards Hybrid Parallelism for Deep Learning Accelerator Array. (Duke, USC)**
----
[2019 HPCA](http://hpca2019.seas.gwu.edu/)

*Linghao Song, Jiachen Mao, Youwei Zhuo, Xuehai Qian, Hai Li, Yiran Chen*

[[论文]](https://arxiv.org/abs/1901.02067)
# Abstract
- **Motivation**
  - large DNN models and datasets incur frequent off-chip memory accesse
  - the training of DNNs is not well-explored in recent accelerator designs
> multiple accelerators as a general architecture: for high throughput and energy efficiency
- **Related work**
  - DNN accelerators, e.g., [Google TPU](https://cloudplatform.googleblog.com/2016/05/Google-supercharges-machine-learning-tasks-with￾custom-chip.html)
  - corresponding standards, architectures, and platforms, e.g.:
    - [Eyeriss][1]: spatial architecture to coordinate dataflow between PEs.
    - [Neurocube][2]: in-memory processing by deploying PEs in hybrid memory cubes (HMCs)
    - [Flexflow][3]: systolic architecture with tiling optimization etc.

[1]: https://ieeexplore.ieee.org/document/7738524
[2]: https://ieeexplore.ieee.org/document/7551408
[3]: https://ieeexplore.ieee.org/abstract/document/7920855/
- **Proposal:**
  - a communication model 
  - hierarchical layer-wise dynamic programming method for partition for each layer
    - partition: feature map tesors(input & output), kernel tensors, gradient tensors, and error tensors
# Background knowledge
- **Inference and training**
  - ![inference](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-Equation%201.PNG) 
  - ![training](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-Equation%202.PNG)
  - ![gradient computation](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-Equation%203.PNG)
- **Parallelisms**
  - data parallelism: Each accelerator holds one part of the partitioned data and a complete copy of the model
    - no communication in data forward and error backward but communications in weight updating
  - model parallelism: The kernel is partitioned into N parts, and feature maps are partitioned accordingly
    - communications in data forward but no communication in error backward and weight updating
# Communication model
- **Parallelism**
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-parallelism.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-interlayer%20communication%20dp%20mp.PNG)
- **Communication**
  - **Intra-Layer**
  
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-intralayer%20communication.PNG)
  - **Inter-Layer**
  
  ![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-interlayer%20communication.PNG)
# LAYER PARTITION
- **Partition Between Two Accelerators**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-BPTA.PNG)

- **Hierarchical Partition**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-HP.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-partition%20hierarchy.PNG)

# HYPAR ARCHITECTURE

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-accelerator%20architecture.PNG)
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-optimized%20parallelism.PNG)
# Results
- **Performance**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-performance.PNG)

- **Energy Efficiency**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-energy.PNG)

- **Communication**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-communication.PNG)

- **Space Exploration**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-space%20exploration.PNG)

- **Scalability**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-scalability.PNG)

- **Topology**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-topology.PNG)

- **Comparison of HYPAR and the Trick**

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-with%20trick.PNG)
