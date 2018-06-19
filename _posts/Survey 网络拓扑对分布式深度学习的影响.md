 分布式消息传递方式汇总![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(7).png) 去中心化的架构2017 NIPS Can Decentralized Algorithms Outperform Centralized Algorithms ? A Case Study for Decentralized Parallel Stochastic Gradient Descent![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(8).png) ![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(9).png) 目前大多数的深度学习系统的网络拓扑。中间节点收集梯度，更新网络的权重。 这篇文章的核心是回答了这个问题：在解决同样的问题上，是否去中心化的架构能比中心化的表现更好。现存的研究都没有比较这一点，都是停留在那种只能用去中心化架构的场景。![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(10).png)![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(11).png) gossip算法

 2018 ICLR Asynchronous Decentralized Parallel Stochastic Gradient descentS-PSGD![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(12).png)PS的 S PSGD消息发送方式

 A-PSGD![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(13).png) AllReduce-SGD![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(14).png)D-PSGD ![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(15).png)  2016 ICDM  Model Accuracy and Runtime Tradeoff in distributed DL ![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(16).png)    ![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(17).png) ![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(18).png) 2018 survey Demystifying![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(19).png)![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(20).png) 2012 NIPS Large Scale Distributed Deep Networks<http://whatbeg.com/2017/01/07/paperreading01.html>![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(21).png)![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(22).png)Downpour SGD的主要思想是，将训练集划分若干子集，并对每个子集运行一个单独的模型副本。模型副本之间的通信均通过中心参数服务器(parameter server, ps)组，该参数服务器组维护了模型参数的当前状态，并分割到多台机器上（例如，如果我们参数服务器组有10个节点，那么每个节点将负责存储和更新模型参数的1/10，如图1所示）
该方法在两个方面体现异步性：
模型副本之间运行独立，在各自负责的数据集（子集）上独立运行
参数服务器(parameter server)组各节点之间同样是独立的，PS片独立运行

考虑Downpour SGD的一个最简单的实现，在处理每个mini-batch（译者注：小型批量）之前，模型副本都会向参数服务器请求最新的模型参数。因为DistBelief框架也是分布在多台机器上，所以其框架的每个节点只需和参数服务器组中包含和该节点有关的模型参数的那部分节点进行通信。在DistBelief副本获得更新后的模型参数后，运行一次mini-batch样本来计算参数的梯度，并推送到参数服务器，以用于更新当前的模型参数值。更新的频度也可以设置，在处理机器失效方面，Downpour SGD比标准（同步）SGD要鲁棒。对于同步SGD来讲，如果一台机器失效，整个训练过程将会延时；但是对于异步SGD来讲，如果某个模型副本的一台机器失效，其他模型副本仍然继续处理样本并更新参数服务器中的模型参数。
但是由于每个参数服务器只更新互斥的一部分参数，副本之间不会进行通信，因此可能会导致参数发散而不利于收敛。
另外一项能极大提高Downpour SGD鲁棒性的技术是使用Adagrad自适应学习速率方法。
通过这种结合，达到了最好的效果（最短的时间内达到了规定的准确率）。
Adagrad方法参见:
J. C. Duchi, E. Hazan, and Y. Singer. Adaptive subgradient methods for online learning and stochastic optimization. Journal of Machine Learning Research, 12:2121–2159, 2011.
中文版的论文翻译中有更详细的内容。![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(23).png) ![img](file:///C:/Users/Ezra/AppData/Local/Temp/enhtmlclip/Image(24).png)          