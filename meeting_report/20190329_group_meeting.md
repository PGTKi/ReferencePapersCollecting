*******
### TNPU: An Efficient Accelerator Architecture for Training Convolutional Neural Networks
1. Q：关于文章Baseline部分提到的cycle accurate simulator的目的与具体实现。   
A：该simulator应该是该项目组自己设计的为TNPU实现的闭源的仿真器，这样得出的对比结果是有所偏差的。
2. Q：paper中在对能耗进行分析时，假设memory access的开销为0，那么在实际的架构中它的功耗是什么情况。   
A：memory access消耗的能量视具体情况而定，一般的话在30%左右，像卷积层由于主要是计算密集型，那么其功耗相对较低，而全连接层由于大量的数据写入读出，那么消耗的功耗就比较大了，这里功耗为0的假设是有所不妥的。
*******
----
### Architectures and Algorithms for User Customization of CNNs
- 主要的创新点：
  - 提出了组合型CNN网络架构，包括比较大的、有通用数据集训练过的、预先设定好的BIE(basic inference engine)和之前参与通用数据集训练、之后由用户数据单独训练的小网络AE(augmenting engine)。
- 进一步的研究问题：
  - BIE、AE的网络结构怎么确定，研究者只说了BIE是将[LeNet-5](http://www.dengfanxin.cn/wp-content/uploads/2016/03/1998Lecun.pdf)的网络调整到输出为62个通道（26\*2+10）的版本，而AE的网络结构并没有解释设置依据。需要在之后的论文阅读中着重了解这一方面的研究。
