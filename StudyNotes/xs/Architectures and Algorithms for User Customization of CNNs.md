---
**Architectures and Algorithms for User Customization of CNNs**
----
# Architectures
- **Basic inference engine(BIE):** fixed(fixed structure, and zero learning rate of the network, freezing its weights and bias values)
  - supplied by vendor of device
    - dedicated hardware accelerator 
    - software implementation
- **Augmenting engine(AE):** re-trained(on-device using a small set of user-specific data)
    - C block
      - learn the user data
    - B block
      - learn to combine theoutputs of the BIE and the C block adaptively
 
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/User%20customization.PNG)

## BIE and AE Network Structure
- BIE: For the BIE we use a version of LeNet-5 [19] adapted to produce 62 outputs. 
- AE for block C : 
  - pooling layer1: 28x28 input to 14x14 
  - convolutional layer 10-channel 5x5
  - pooling layer1: 10 channels to 5x5
  - reshape (flattening) layer: to a 250-element vector
- AE for Block B: 
  - combines the 62-element vector of the BIE with the vector from C
  - apply a ReLU activation function 
  - a fully-connected layer connects the 312 inputs to the 62 output channels.
  
![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/The%20BIE%20(left%20channel)%20and%20the%20AE%20(blocks%20B%20and%20C)%20for%20NIST.PNG)


- **Cosrse-grained reconfigurable array(CGRA):** Samsung Reconfigurable Processor (SRP): 
32-bit floating-point, 4x4 heterogeneous PEs, 320KB of on-chip data SRAM
  - programmable to a variety of distinct tasks 
  - excel at data-flow-style processing of loops

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Schematic%20of%20a%20coarse-grained%20reconfigurable%20array%20processor.PNG)

    - runs bare-metal: not available for runtime environments (i.e., Python) or libraries (such as the C standard library or math libraries)
      - simple deep neural network framework written in pure C
    - the latency of the load instruction is set tO 16 cycles from the original six 
      - 4 for actual memory access latency and 12 for *data memory queue*(DMQ)

# Code Optimizations
**compiler-level optimizations**
- loop unrolling
- loop fusion
- loop interchange

# Workflow
> inputs read: input dataset, input labels, network specification
>> parses the network specification: to construct the desired network as a C program
>>> executes this program: performing *forward propagationn*, *back propagation* and *weight updates*

![](https://github.com/PGTKi/ReferencePapersCollecting/blob/master/StudyNotes/xs/pictures/Training%20flow.PNG)

# Results
- Accuracy (based on NIST'19: a collection of over 730000 letters and digits for training and 82000 for testing)
  - LeNet-5: for NIST'19 - 82.1% for user data - 76.3%
  - proposed: 93.2%
- Mapping the code to a CGRA
  - 45x speed up 
- energy consumption
  - 49-fold reduction over ARMv7 processor
  - 3-fold reduction over 3-way VLIW processor


