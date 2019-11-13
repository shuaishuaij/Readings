# 论文解析

## 重点语句

### Abstract

* A major reason lies in that the discrete outputs from the generative model make it difficult to pass the gradient update from the discriminative model to the generative model.

* the discriminative model can only assess a complete sequence, while for a partially generated sequence, it is nontrivial to balance its current score and the future one once the entire sequence has been generated.

* Modeling the data generator as a stochastic policy in reinforcement learning (RL), SeqGAN bypasses the generator differentiation problem by directly performing **gradient policy update**.

### Introduction

* the maximum likelihood approaches suffer from so-called exposure bias in the inference
stage: the model generates a sequence iteratively and predicts next token conditioned on its previously predicted ones
that may be never observed in the training data.

* Scheduled sampling (SS), where the generative model is partially fed with its own synthetic data as prefix (observed tokens) rather than the true data when deciding the next token in the training stage. 

*  SS is an inconsistent training strategy and fails to address the problem fundamentally

* Another possible solution
of the training / inference discrepancy problem is to build
the loss function on the entire generated sequence instead
of each transition.

* a task specific sequence score/loss, bilingual evaluation understudy (BLEU) (Papineni et al. 2002),
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