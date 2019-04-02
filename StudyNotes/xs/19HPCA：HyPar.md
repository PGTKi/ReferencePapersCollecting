----
**HyPar: Towards Hybrid Parallelism for Deep Learning Accelerator Array. (Duke, USC)**
----
[[论文]](https://arxiv.org/abs/1901.02067)
# Abstract
- Motivation
  - large DNN models and datasets incur frequent off-chip memory accesse
  - the training of DNNs is not well-explored in recent accelerator designs
> multiple accelerators as a general architecture: for high throughput and energy efficiency
- Related work
  - DNN accelerators, e.g., Google TPU
  - corresponding standards, architectures, and platforms, e.g.:
    - Eyeriss: spatial architecture to coordinate dataflow between PEs.
    - Neurocube: in-memory processing by deploying PEs in hybrid memory cubes (HMCs)
    - Flexflow: systolic architecture with tiling optimization etc.
- Proposal:
  - a communication model 
  - hierarchical layer-wise dynamic programming method for partition for each layer
    - partition: feature map tesors(input & output), kernel tensors, gradient tensors, and error tensors
# Background knowledge
- inference and training
  - ![inference](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-Equation%201.PNG) 
  - ![training](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-Equation%202.PNG)
  - ![gradient computation](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/19HPCA-HyPar-Equation%203.PNG)


# Communication model
- 
