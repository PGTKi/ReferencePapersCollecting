# Architectures and Algorithms for User Customization of CNNs
----
## Architectures
- **Basic inference engine(BIE):** fixed(fixed structure, and zero learning rate of the network, freezing its weights and bias values)
  - supplied by vendor of device
    - dedicated hardware accelerator 
    - software implementation
    
- **Augmenting engine(AE):** re-trained(on-device using a small set of user-specific data)
    - c block
      - learn the user data
    - B block
      - learn to combine theoutputs of the BIE and the C block adaptively
- **Cosrse-grained reconfigurable array(CGRA):** Samsung Reconfigurable Processor (SRP): 32-bit floating-point, 4x4 heterogeneous PEs, 320KB of on-chip data
SRAM
  - programmable to a variety of distinct tasks 
  - excel at data-flow-style processing of loops
