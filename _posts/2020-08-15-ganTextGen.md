---
layout: post
title: Generative Adversarial Networks in Text Generation Research
published: true
---
>In this post I will summarize some important research on text generation using Generative Adversarial Networks.  This is not a comprehensive review.  The first focus will be on the application of Wasserstein GAN in text generation.

## Brief review of generative adversarial networks

![gan model]({{ site.baseurl }}/images/2020-08-15-ganTextGen/GAN.png)

### Minimax loss function:
![minimax cost function]({{ site.baseurl }}/images/2020-08-15-ganTextGen/minimaxcost.png)

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



#### earth moving metrics

#### Why Wasserstain


### application of Wasserstain distance in GAN text generation
