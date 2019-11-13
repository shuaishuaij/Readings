# 论文解析

## 重点语句

### Abstract

* A major reason lies in that the discrete outputs from the generative model make it difficult to pass the gradient update from the discriminative model to the generative model.

* the discriminative model can only assess a complete sequence, while for a partially generated sequence, it is non-trivial to balance its current score and the future one once the entire sequence has been generated.

* Modeling the data generator as a stochastic policy in reinforcement learning (RL), SeqGAN bypasses the generator differentiation problem by directly performing **gradient policy update**.

### Introduction

* the maximum likelihood approaches suffer from so-called exposure bias in the inference
stage: the model generates a sequence iteratively and predicts next token conditioned on its previously predicted ones
that may be never observed in the training data.

* Scheduled sampling (SS), where the generative model is partially fed with its own synthetic data as prefix (observed tokens) rather than the true data when deciding the next token in the training stage. 

*  SS is an inconsistent training strategy and fails to address the problem fundamentally

* Another possible solution
of the training / inference discrepancy problem is to build
the loss function on the entire generated sequence instead
of each transition.

* a task specific sequence score/loss, bilingual evaluation understudy (BLEU) (Papineni et al. 2002),
can be adopted to guide the sequence generation.

* In GAN a discriminative
net D learns to distinguish whether a given data instance is
real or not, and a generative net G learns to confuse D by generating high quality data.

* two problems:
    1.  GAN is designed for generating real-valued, continuous data.
    The reason is that in GANs, the generator starts with random sampling first and then a determistic
transform, govermented by the model parameters.
    2.  GAN can only give the score/loss for
an entire sequence when it has been generated; for a partially
generated sequence, it is non-trivial to balance how good as
it is now and the future score as the entire sequence.

* To address the 2 problems:
    1. Consider the sequence generation procedure as a sequential decision making process.
    2. The generative model is treated as an agent of reinforcement learning (RL); the state is the generated tokens so far and the action is the next token to be generated.
    3. we employ a discriminator to
evaluate the sequence and feedback the evaluation to guide
the learning of the generative model.
    4. To solve the problem
that the gradient cannot pass back to the generative model
when the output is discrete, we **regard the generative model
as a stochastic parametrized policy**. In our policy gradient,
we employ **Monte Carlo (MC) search** to approximate the
**state-action value**. We directly train the policy (generative
model) via **policy gradient** (Sutton et al. 1999), which naturally avoids the differentiation difficulty for discrete data in a conventional GAN.

*  In **3 realworld tasks**, i.e. **poem generation**, **speech language generation** and **music generation**, SeqGAN significantly outperforms the compared baselines in various metrics including
human expert judgement.

### Related Work

* GAN bypasses the difficulty of maximum likelihood learning and has gained striking successes in natural image generation (Denton et al. 2015). However, little progress has
been made in applying GANs to sequence discrete data generation problems, e.g. natural language generation (Huszar 2015). This is due to the generator network in GAN is designed to be able to adjust the output continuously, which
does not work on discrete data generation (Goodfellow
2016).

* The most popular way of training RNNs is to maximize the likelihood of
each token in the training data whereas (Bengio et al. 2015)
pointed out that the discrepancy between training and generating makes the maximum likelihood estimation suboptimal and proposed scheduled sampling strategy (SS)

* **(Huszar 2015)** theorized that the objective function underneath SS is improper and explained the reason why GANs tend to generate natural-looking samples in theory.

* **Consequently, the GANs have great potential but are not practically feasible to discrete probabilistic models currently.**

* For most practical sequence generation tasks, e.g. machine translation (Sutskever,
Vinyals, and Le 2014), **the reward signal is meaningful only
for the entire sequence**, for instance in the game of Go (Silver et al. 2016), the reward signal is only set at the end of the
game. 

*  Here in our work , **a reward signal is provided by
the discriminator at the end of each episode via Monte Carlo
approach**, and the generator picks the action and learns the
policy using estimated overall rewards.

### Model

![SeqGAN](https://github.com/shuaishuaij/Readings/blob/master/NLP-GAN-%E5%AF%B9%E6%8A%97%E8%AE%AD%E7%BB%83/SeqGAN/pics/seqGAN1.PNG)