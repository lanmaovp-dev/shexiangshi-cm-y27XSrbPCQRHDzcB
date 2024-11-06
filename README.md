

> 来源：晓飞的算法工程笔记 公众号，转载请注明出处


**论文: Agglomerative Token Clustering**


![](https://developer.qcloudimg.com/http-save/6496381/db384c69fa51ab1e76e06217bc9fe402.png)


* **论文地址：[https://arxiv.org/abs/2409\.11923](https://github.com)**
* **论文代码：[https://github.com/JoakimHaurum/ATC](https://github.com):[wgetCloud机场](https://tabijibiyori.org)**


# 创新点




---


* 提出了层次`token`聚类（`Agglomerative Token Clustering`，`ATC`），这是一种新型的无参数层次合并的`token`减少方法。
* 基于`ATC`，在图像分类、图像合成，以及目标检测和分割任务上实现了最先进的性能，超越了所有其他`token`减少方法，包括基于合并的和基于修剪的`token`减少方法。
* 在图像分类和目标检测与分割任务中，`ATC`可以在未经过任何微调的情况下（即开箱即用），达到与之前微调的最先进性能相当的效果。


# 内容概述




---


层次`token`聚类（`Agglomerative Token Clustering`，简称`ATC`）是一种新型的`token`合并方法，在图像分类、图像合成以及目标检测与分割任务中始终优于以往的`token`合并和修剪方法。`ATC`通过自下而上的层次聚类来合并簇，而无需引入额外的可学习参数。


在所有任务中，`ATC`都实现了最先进的性能。在不进行微调的情况下，甚至可以与之前的最先进技术相媲美。`ATC`在低保留率下尤其有效，此场景仅保留了少量的`token`，而保持任务性能尤其困难。


# 层次`token`聚类




---


与之前的`token`合并方法类似，`ATC`的目标是合并冗余`token`，同时保持或提升`ViT`模型的性能。在`ViT`块的自注意力和多层感知机（`MLP`）模块之间插入`token`合并操作，这与之前的基于合并的方法是一致的，比如`ToMe`。


![](https://developer.qcloudimg.com/http-save/6496381/2df50bee9846e3f3ffbe56162306c5b8.png)


层次聚类是一种经典的自下而上的层次聚类方法，其中每个元素最初都是其自身的聚类。通过根据某种连结函数和距离度量 D(⋅) 迭代比较聚类，将两个最接近的聚类在每次迭代中合并。这一过程会持续进行，直到满足某个停止标准，例如所需聚类的数量（形成静态缩减方法），或者聚类之间的最小距离（形成动态缩减方法）。


论文考虑静态缩减场景，使用余弦距离作为距离度量 D(⋅) ，并使用自注意力模块的键作为`token`特征。连结函数的选择对元素的聚类方式会有很大影响，主要有三种最常见的连结函数：单个，完整和平均。


D(I,J)single\=min\\begin{equation}
D(I,J)^{\\text{complete}} \= \\max\_{i\\in I,\\ j\\in J} D(i,j)
\\end{equation}
\\begin{equation}
D(I,J)^{\\text{average}} \= \\frac{1}{\|I\|\|J\|}\\sum\_{i\\in I}\\sum\_{j\\in J}D(i,j)
\\end{equation}
其中 I 和 J 是包含元素 i \\in I 和 j \\in J 的聚类。


在达到停止标准之后，对每个聚类中的`token`进行平均，以获得更新的聚类表示。然而，随着`token`的合并，它们代表的不止一个输入图像块。为了更好地利用能够捕捉更大空间范围的`token`，使用加权平均作为聚类表示，并在自注意力模块中使用成比例的注意力。


# 主要实验




---


![](https://developer.qcloudimg.com/http-save/6496381/fad53027848e92f5e295d4c9183c5260.png)


![](https://developer.qcloudimg.com/http-save/6496381/ed455f3963eccfb5e0455f12a90688b1.png)


![](https://developer.qcloudimg.com/http-save/6496381/71e7854f091941bc72d13d3da77a70dd.png)


 
 
 



> 如果本文对你有帮助，麻烦点个赞或在看呗～
> 更多内容请关注 微信公众号【晓飞的算法工程笔记】


![work-life balance.](https://upload-images.jianshu.io/upload_images/20428708-7156c0e4a2f49bd6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
