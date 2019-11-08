## 基础知识巩固

#### 转载
RL-GAN For NLP: 强化学习在生成对抗网络文本生成中扮演的角色：
https://zhuanlan.zhihu.com/p/29168803

通俗理解生成对抗网络GAN: 
https://zhuanlan.zhihu.com/p/33752313

#### 对minmax的理解:

这里的<img src="http://www.forkosh.com/mathtex.cgi? V(G, D)">相当于表示真实样本和生成样本的差异程度。
先看 <img src="http://www.forkosh.com/mathtex.cgi? \max _{D} V(D, G)"> 。这里的意思是固定生成器G，尽可能地让判别器能够最大化地判别出样本来自于真实数据还是生成的数据。
再将后面部分看成一个整体令 <img src="http://www.forkosh.com/mathtex.cgi? L=\max _{D} V(D, G)"> ，看 <img src="http://www.forkosh.com/mathtex.cgi? \min _{G} L">，这里是在固定判别器D的条件下得到生成器G，这个G要求能够最小化真实样本与生成样本的差异。
通过上述min-max的博弈过程，理想情况下会收敛于生成分布拟合于真实分布。

### Wasserstein-divergence

### Gumbel-softmax，模拟Sampling的softmax

一篇来自华威大学+剑桥大学的工作把改进GAN用于离散数据生成的重心放在了修改softmax的output这方面。Sampling 操作中的 argmax() 函数将连续的softmax输出抽取成离散的成型输出，从而导致Sampling的最终output是不可微的，形成GAN对于离散数据生成的最大拦路虎，既然不用Sampling的时候，output与真实分布不重叠，导致JS散度停留于固定值 log2，如果用了Sampling的话，离散数据的正常输出又造成了梯度 Back-Propagation 上天然的隔阂。

既然如此，论文的作者寻找了一种可以高仿出Sampling效果的特殊softmax，使得softmax的直接输出既可以保证与真实分布的重叠，又能避免Sampling操作对于其可微特征的破坏。它就是“耿贝尔-softmax”（Gumbel-Softmax），Gumbel-Softmax早先已经被应用于离散标签的再分布化[15]（Categorical Reparameterization），在原先的Sampling操作中， argmax() 函数将普通softmax的输出转化成One-hot Vector：

    y = One_Hot( argmax( softmax(h) ) )

而Gumbel-Softmax略去了 One_Hot(argmax()), 能够直接给出近似Sampling操作的输出：

    y = softmax(1/τ(h + g))
精髓在于这其中的“逆温参数” τ. 当τ趋于0时, 上述分布约等于原来的 One_Hot(argmax()), 反之当其趋于无穷大时, 上述分布约等于均匀分布. 则τ作为这个特殊softmax中的一个超参数，给予一个较大的初始值，通过训练学习逐渐变小，向 0 逼近

### Reinforcement Learning
自AlphaGo使得强化学习猛然进入大众视野以来，大部分对于强化学习的理论研究都将游戏作为主要实验平台，这一点不无道理，强化学习理论上的推导看似逻辑通顺，但其最大的弱点在于，基于人工评判的奖励 Reward的获得，让实验人员守在电脑前对模型吐出来的结果不停地打分看来是不现实的，游戏系统恰恰能会给出正确客观的打分（输/赢 或 游戏Score）。基于RL的对话生成同样会面对这个问题，研究人员采用了类似AlphaGo的实现方式（AI棋手对弈）——同时运行两个机器人，让它们自己互相对话，同时，使用预训练（pre-trained）好的“打分器”给出每组对话的奖励得分，关于这个预训练的“打分器” R ，可以根据实际的应用和需求自己DIY。 

### SeqGAN 和 Conditional SeqGAN
RL + GAN for Text Generation，SeqGAN[17]站在前人RL Text Generation的肩膀上，可以说是GAN for Text Generation中的代表作。上面虽然花了大量篇幅讲述RL ChatBot的种种机理，其实都是为了它来做铺垫。试想我们使用GAN中的判别器D作为强化学习中奖励 Reward 的来源，假设需要生成长度为T的文本序列，则对于生成文本的奖励值

    \tilde{R}_{\theta}\left(s_{1: t-1}\right)=\frac{1}{N} \sum_{i=1}^{N} D\left(s_{1: t-1}+y^{i}\right), \quad y^{i} \in M C_{\theta}\left(s_{1: t-1} ; N\right)

### IRGAN 信息检索模型之间的对抗游戏























