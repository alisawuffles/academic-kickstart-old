---
title: Incorporating Music Knowledge in Continual Dataset Augmentation for Music Generation
date: 2020-07-18
math: true
diagram: true
markup: mmark
---
*Alisa Liu, Alex Fang, Gaëtan Hadjeres, Prem Seetharaman, Bryan Pardo*

This post accompanies our poster presentation at the Machine Learning for Media Discovery Workshop at ICML 2020.

## Examples from augmentative generation
Here are example generations from a model trained through the augmentative generation method we introduce in this paper, along with Bach chorales and the generations from two baselines.

### Bach
Two original Bach chorales!

BWV 297: {{< audio mp3="../../audio/bach/models/bach225.mp3" >}}
BWV 194.6: {{< audio mp3="../../audio/bach/models/bach249.mp3" >}}


### Augmentative generation
During training, augment the training dataset with all generated chorales that receive a grade better than 25% of Bach chorales.

{{< audio mp3="../../audio/bach/models/aug-gen8.mp3" >}}
{{< audio mp3="../../audio/bach/models/aug-gen26.mp3" >}}

### Baseline-none
Do not include any generated chorales in the training dataset. This is equivalent to training a model on only Bach chorales, without dataset augmentation!

{{< audio mp3="../../audio/bach/models/base8.mp3" >}}
{{< audio mp3="../../audio/bach/models/base10.mp3" >}}

### Baseline-all
During training, augment the training dataset with all generated chorales, regardless of quality.
{{< audio mp3="../../audio/bach/models/baseline5.mp3" >}}
{{< audio mp3="../../audio/bach/models/baseline6.mp3" >}}
