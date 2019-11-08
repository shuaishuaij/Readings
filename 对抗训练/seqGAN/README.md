## 基础知识巩固

#### 转载
RL-GAN For NLP: 强化学习在生成对抗网络文本生成中扮演的角色：
https://cloud.tencent.com/developer/article/1087101

通俗理解生成对抗网络GAN: 
https://zhuanlan.zhihu.com/p/33752313

#### 对minmax的理解:

这里的<img src="http://chart.googleapis.com/chart?cht=tx&chl=$V(G, D)$" style="border:none;">相当于表示真实样本和生成样本的差异程度。
先看 [公式] 。这里的意思是固定生成器G，尽可能地让判别器能够最大化地判别出样本来自于真实数据还是生成的数据。
再将后面部分看成一个整体令 [公式] = [公式] ，看 [公式]，这里是在固定判别器D的条件下得到生成器G，这个G要求能够最小化真实样本与生成样本的差异。
通过上述min max的博弈过程，理想情况下会收敛于生成分布拟合于真实分布。


























