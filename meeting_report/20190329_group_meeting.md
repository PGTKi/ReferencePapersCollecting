*******
### TNPU: An Efficient Accelerator Architecture for Training Convolutional Neural Networks
1. Q：关于文章Baseline部分提到的cycle accurate simulator的目的与具体实现。   
A：该simulator应该是该项目组自己设计的为TNPU实现的闭源的仿真器，这样得出的对比结果是有所偏差的。
2. Q：paper中在对能耗进行分析时，假设memory access的开销为0，那么在实际的架构中它的功耗是什么情况。   
A：memory access消耗的能量视具体情况而定，一般的话在30%左右，像卷积层由于主要是计算密集型，那么其功耗相对较低，而全连接层由于大量的数据写入读出，那么消耗的功耗就比较大了，这里功耗为0的假设是有所不妥的。
*******
