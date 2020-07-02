---
title: Ensemble model for audio source separation
summary: Built an ensemble model for audio source separation that can handle mixtures whose source domain is unknown, using a confidence measure to mediate among domain-specific models based on deep clustering.
tags:
- research
- audio
date: "2019-10-01"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption:
  focal_point:
  preview_only: true

# links:  
# - icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
# url_code: ""
# url_pdf: ""
# url_slides: ""
# url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides: example
---

Built an ensemble model for audio source separation that can handle mixtures whose source domain is unknown, using a confidence measure to mediate among domain-specific models based on deep clustering. We derived a confidence measure based on the clusterability of the embedding space which approximates the separation quality without ground-truth comparison.

<!-- As humans, we are able to distinguish individual sound sources in complex auditory scenes, as evidenced by our daily encounters with sound. In a crowded coffee shop, we can selectively attend to a friend speaking to us. When we step outdoors, we can identify a passing car, birds chirping, and distant voices. Listening to pop music, we can distinguish the vocals and the instrumental background. As we switch from one audio environment to the next, our brains actually shift among many different grouping mechanisms, such as direction of arrival (grouping sounds from the same spatial location), common fate (grouping sounds that start, stop, and move together), timbral similarity (grouping sources like voices and instruments by timbre), and so on.

In the same way that different audio mixtures require humans to leverage different cues, source separation models are traditionally trained to separate data of a specific domain, and are likely to fail when applied to mixtures unlike the training data.

To see this, consider three models, trained on music mixtures, speech mixtures, and environmental sound mixtures, and an audio clip from each of these three domain. Below, each mixture is separated by each model. We can verify that the separation quality is highest when a mixture is separated by a model trained on the same domain.

[table of audio clips here]

This fundamentally limits how source separation models can be deployed, requiring a user to know about a model's training to select the correct one for a given mixture. Imagine a hearing aid that automatically switches models when a user moves from the busy cafe to an outdoors environment, in the same way that our brain shifts among auditory cues. That is, we want a general source separation model that can handle mixtures where the source domain is unknown. To do this, we automate selection of the appropriate domain-specific model for a given audio mixture, via a confidence measure that does not require ground truth to estimate separation quality.

## A confidence-based ensemble
We automate selection of the appropriate domain-specific deep clustering source separation model for an audio mixture of unknown domain. We present a confidence measure that does not require ground truth to estimate separation quality, given a model and an audio mixture. We use this confidence measure to automatically select the best model output for the mixture.

### Deep clustering
We apply our method to **deep clustering** source separation networks. In deep clustering, a neural network is trained to map each time-frequency bin in a magnitude spectrogram of an audio mixture to a higher-dimensional embedding, such that bins that primarily contain energy from the same source are near each other and bins whose energy primarily come from different sound sources are far from each other. Given a good mapping, the assignment of bin to source can then be determined by a simple clustering method. All members of the same cluster are assigned to the same source. Because deep clustering performs separation via clustering, we develop a confidence measure that relies on the embedding space, with the core insight that the embedding space produced by deep clustering is indicative of the performance of the algorithm.

![deep-clustering](../../img/deep-clustering.png)

### Confidence measure
Define $X$ as the set of embeddings for every time-frequency point in an audio mixture, where $x\_i$ is the embedding of one point. $X$ is partitioned into $K$ clusters $C\_{k}$, that is, $X = \bigcup_{k=1}^K C_k$. Consider a data point $x_i$ assigned to cluster $C\_k$.

#### Silhouette score
The intercluster distance $a(x\_i)$ captures how much separation exists between the clusters. Specifically, it is the mean distance $x\_i$ and all the points in the nearest cluster $C\_\ell$.
$$a(x\_i) = \frac{1}{|C\_k| - 1} \sum_{\substack{x\_j \in C\_k,\\ x\_i \neq x\_j}} d(x\_i, x\_j)$$

The intracluster distance $b(x\_i)$ captures how dense the clusters are.  Specifically, it is the mean distance (using distance function $d$) between $x\_i$ and all other points in $C\_k$.
$$b(x\_i) = \min\_{\ell \neq k} \frac{1}{|C\_\ell|} \sum\_{x\_j \in C\_\ell} d(x\_i, x\_j)$$

Compute the _silhouette score_ of $x\_i$ as

$$S(x\_i) = \frac{b\left(x\_i\right) - a\left(x\_i\right)}{\max\left(a(x\_i), b(x\_i)\right)}$$

Note $S(x_i)$ ranges from $-1$ to $1$.

#### Posterior strength
For every point $x\_i$ in a dataset $X$, the soft K-means clustering algorithm produces $\gamma\_{ik} \in [0, 1]$, which indicates the membership of the point $x\_i$ in some cluster $C\_k$, also called the \textit{posterior} of the point $x\_i$ in regards to the cluster $C\_k$. The closer that $\gamma\_{ik}$ is to $0$ (not in the cluster) or $1$ (in the cluster), the more sure the assignment of that point. We compute the _posterior strength_ of $x\_i$ as follows:

$$P(x\_i) = \frac{K \left(\max\limits\_{k \in [0, ..., K]} \gamma\_{ij}\right) - 1}{K - 1}$$

The equation maps embeddings that have a maximum posterior of $\frac{1}{K}$ (equal assignment to all clusters) to $0$, and points that have a maximum posterior of $1$ to $1$.

The confidence measure $C(X)$ combines the silhouette score $S(X)$ and posterior strength $P(X)$ through multiplication so that it is high only when both are high. That is, $C(X)=S(X)P(X)$.

Below is a visualization of the confidence measure as applied to the distribution of points in a mixture produced by three trained deep clustering networks, each trained on a different domain. The input is a music mixture. The speech (left) and environmental (right) models return distributions with no clear clusters. The music model (middle) returns a more clusterable distribution, which is reflected by a higher confidence score.

![embedding-visualization](../../../img/embedding-visualization.png)

## Experiments
For each domain that we considered - separating two speakers in a speech mixture, separating vocals from accompaniment in music mixtures, and separating environmental sounds from one another - we train 3 deep clustering networks with identical setups. Each network has 2 BLSTM layers with 300 hidden units each and an embedding size of 20 with sigmoid activation. We trained each network for 80 epochs using the Adam optimizer (learning rate was 2e-4).

### Correlation with SDR
We first demonstrate that the confidence measure correlates well with source to distortion ratio (SDR), a widely used measure of source separation quality. Below, we see a clear relationship between confidence and performance for the speech model as applied to the speech test mixtures. Further, we see that both confidence and performance are a function of the mixture \textit{type}. Same-sex mixtures are harder to separate due to the frequency overlap between the speakers. This is reflected in Figure . For the other domains, we also observe strong correlations. A linear fit between the confidence measure and SDR applied to music mixtures separated by a deep cluster model trained on music mixtures returned an r-value of $0.46$ for vocals and $0.63$ for instrumentals. The linear fit for environmental sounds separated by a model trained on environmental sounds had an r-value of $.70$.

### Performance of the confidence-based ensemble
Then we evaluate the performance of our confidence-based ensemble compared to an oracle ensemble, a random ensemble, and each domain-specific model on general mixtures. The following table shows the performance of various approaches to separating each dataset. Values in the table represent the mean separation quality of each model (based on SDR) when evaluated on all 9,000 test mixtures.

| Approach              | Speech | Music  | Environmental |
|-----------------------|:------:|:------:|:-------------:|
| Ensemble -- oracle     | 8.37   | 6.55   | 12.21         |
| Ensemble -- random     | 4.86   | 4.25   | 2.82          |
| Ensemble -- confidence |**7.61**|**6.47**|**10.52**      |
| Speech model          | 8.29   | 2.06   | 3.03          |
| Music model           | 1.43   | 6.50   | 2.57          |
| Environmental model   | 2.16   | 1.77   | 11.94         |

The top three rows show the performance of three ensemble approaches, which switch between the three domain-specific models via different strategies. The oracle ensemble switches between them with knowledge of the true performance of each model. This is the upper bound for any switching system. The random ensemble randomly selects the model to apply to a given mixture, with equal probability. The confidence ensemble uses our confidence measure to select between the models. For each mixture, all three models are run and confidence measures are computed. The output from the model with the highest confidence is then chosen as the separation. The confidence-based ensemble significantly outperforms the random ensemble. In the case of music mixtures, the confidence-based model achieves almost oracle performance, with mean SDR of $6.47$ compared to $6.55$.

The bottom three rows show the performance of individual domain-specific models on all of the domains we consider. Predictably, every model shows poor performance on domains it was not trained on.

## Conclusion
We have presented a method for effectively combining the output of multiple deep clustering models by switching between them based on mixture domain in an unsupervised fashion. Our method works by analyzing the embedding produced by each deep clustering network to produce a confidence measure that is predictive of separation performance. This confidence measure can be applied to ensembles of any clustering-based separation algorithms. -->
