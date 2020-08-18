---
layout: post
title: Generative Adversarial Networks in Text Generation Research
published: true
---
>In this post I will summarize some important research on text generation using Generative Adversarial Networks.  This is not a comprehensive review.  The first focus will be on the application of Wasserstein GAN in text generation.

## Brief review of generative adversarial networks

![gan model]({{ site.baseurl }}/images/2020-08-15-ganTextGen/GAN.png)

### Minimax loss function:
![minimax cost function]({{ site.baseurl }}/images/2020-08-15-ganTextGen/minmaxcost.png)

The discriminator wants to maximize L, while the generator wants to minimize L.  They do this with gradient decent, in alternation.

### Success of GAN in image generation



## Challenges for using GAN in text generation
* The generator can produce text sample for the discriminator to compare with real samples.  But as the produced samples are discretely data, errors can not be back propagated through to the generator.
* One way to deal with the problem of discrete data is to use the continous latent space.  This creates another problem of disjoint distribution.
* Exposure bias.  In text generation, if RNN is used, the generator is often trained with real data for each step, yet in inferencing time, it only depends on what it has generated before each step.  Small errors in each step can accumulate.

### Reinforcement learning in GAN generation of text

### Autoencoder in GAN generation of text

### output softmax from generation.  Problems with this approach
We can use the softmax output layer of the generator, instead of the one-hot text output.  This way, we will be able to back propagate errors back to the generator.  There are two problems:
* the produced softmax matrix is disjoint with the real data, and thus is very easy for the discriminator to learn
* gradient vanishing problem for JSD

### Wasserstain distance

#### Jenson-Shannon Divergence and the minimax loss
In traiing, the discriminator attempts to maximize minimax loss.  The better ddiscriminator is trained, the more accurate the gradient is for the generator.

![minimax loss for discriminator]({{ site.baseurl }}/images/2020-08-15-ganTextGen/lossford.png)

With some simple derivative computation we can get the optimum discriminator:

![optimum discriminator]({{ site.baseurl }}/images/2020-08-15-ganTextGen/optimumD.png)

This is Jenson-Shannon Divergence:

![jsd]({{ site.baseurl }}/images/2020-08-15-ganTextGen/jsd.png)

Optimizing minimax loss is computing JSD!

![optimum minimax is jsd]({{ site.baseurl }}/images/2020-08-15-ganTextGen/minimaxlossisjsd.png)

#### gradient vanishing problem

![vanishing gradient]({{ site.baseurl }}/images/2020-08-15-ganTextGen/vanishingGradient.png)




#### earth mover distance
Wasserstain distance is also called earth mover distance as it represents the minimum mass in probability that needs to be moved, in order to transform from one distribution to the othre.

![wasserstain]({{ site.baseurl }}/images/2020-08-15-ganTextGen/wassernstein.png)

Π(pr,pg) is the set of all possible joint probability distributions between pr and pg, s.t. ∑xγ(x,y)=pg(y) and ∑yγ(x,y)=pr(x). 

#### Intuition for the Wasserstain equation

Wasserstan distance pickes the smallest effort to move the earth in a pile (the distribution), so that the pile goes from one shape to the other.  

In the simplified discrete bucket case, we can generalize the earth movement as the redistribution of the earth from each of the buckets to all buckets.

![earth mover]({{ site.baseurl }}/images/2020-08-15-ganTextGen/earthmover.png)

If we put this into matrix form, we get rows that represent the redistribution from one bucket to all buckets (including itself); and we get columns that represent each bucket receiving from all buckets (including itself).  The results is that, the sum of each row is the amount of earth in the original distribution, and the sum of each column is the earth in the target distribution.

Indices in such a matrix is the fromBucket number and the toBucket number respectively, and the relative distance, abs (\|x \- y\|) , times the amount of earth for that transfer, is the work to be done for this transfer.  And the element-wise sum of the matrix is the total work.  

In all such matrices, the one having the smallest element wise sum of earth X distance, is the minimum amount of work for transforming one pile to the other.

Likewise, in all joint distributions between two distributions (pr, pg), in which the marginal probability along one way is pr, and the marginal probability along the other way is pg, we can find the matrix of least transformation effort with the Wasserstain distance equation.


#### Why Wasserstain

Let look at an example and see how Wasserstain distance solves the vanishing gradient problem.

![why wasserstain]({{ site.baseurl }}/images/2020-08-15-ganTextGen/whyWasserstain.png)


![divergenceComparison]({{ site.baseurl }}/images/2020-08-15-ganTextGen/divergenceComparison.png)


![wasserstainVsJSD]({{ site.baseurl }}/images/2020-08-15-ganTextGen/wasserstainVsJSD.png)



#### How to compute Wasserstain

**Lipschitz continuity
R -> R function f is K-Lipschitz continuous if:
![Lipschitz]({{ site.baseurl }}/images/2020-08-15-ganTextGen/Lipschitz.png)

For all x1, x2.

**Kantorovich-Rubinstein duality ([derivation](https://vincentherrmann.github.io/blog/wasserstein/))

![Kantorovich-Rubinstein duality]({{ site.baseurl }}/images/2020-08-15-ganTextGen/krduality.png)


∥f∥L≤K means f is K-Lipschitz continuous
You can use a discriminator to learn f.


![wasserstain gradient]({{ site.baseurl }}/images/2020-08-15-ganTextGen/wassernsteinGradientForD.png)

This is a well defined gradient even when g and r are disjoint.


### application of Wasserstain distance in GAN text generation







