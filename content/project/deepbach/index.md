---
title: Augmentative generation for Bach chorales
summary: Using heuristic evaluations of generated music to train a Bach chorale generator on high-quality generations
tags:
- research
- class project
- ongoing
- music
date: "2019-11-01"

image:
  caption:
  focal_point:
  preview_only: True

authors: ['admin', 'alex']

links:
# - icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
url_code: "https://github.com/asdfang/CC-AugmentedDeepBach"
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

Deep learning has become the state-of-the-art approach for music generation, but these networks require massive data. For music generation systems on a specific music domain, such as Bach chorales, Mozart sonatas, or Beatles hits, training data is limited to the real work of musicians during their lifetime. Researchers commonly approach this problem by artificially augmenting the dataset with synthetic data, a technique known as _dataset augmentation_ [\[1\]]({{< relref "#challenges_directions_cite">}}). In the musical domain, a natural way is to transpose music examples in all keys, or to segment examples into many shorter fragments. However, these all suffer musical limitations: the original key signature is deliberately chosen by the composer and typically associated with certain emotions and expressive qualities, and segmenting examples prevents a network from being able to learn the long-term coherence of human-written music.

We propose an alternative method for data augmentation, which we call _augmentative generation_. After training a base model on the original dataset, we generate some output from the model, filter it by a hand-crafted score function, and feed it back to the model as new training data to update the model. These generate-update iterations can be continued until it stops improving the model.

We try this method with chorale generation in the style of J.S. Bach, using the DeepBach model [\[2\]]({{< relref "#deepbach_cite" >}}). For Bach chorale generation, gold-standard data is inherently limited to the 389 chorales Bach wrote in his lifetime. While this is considered a large output for a single form for a composer, 389 examples are often not enough to train machine learning models. Moreover, the unique sound quality of each key signature is especially important in Baroque compositions, which are are written with the equal temperament tuning system in mind, giving each key a distinctive character. This makes transposition an undesirable method of dataset augmentation and motivates our proposed solution.

While augmentative generation can be applied to any generation system where training data is limited, the score function must be handcrafted for each domain. We argue that this is a good thing, since a limitation of deep learning techniques compared to handcrafted models is that they do not offer direct ways of controlling generation. But given that we know some things about what widely accepted music (like Bach chorales or Mozart sonatas) should sound like, we believe that a handcrafted function allows musicians to incorporate important domain knowledge into the music generation task, while still using the representational capacity of a neural network. That is, rather than enforcing things about our output, we can enforce things about the input that our model is trained on.

# Proposed method
First we train the base model on the original 351 Bach chorales. Compared to the original DeepBach paper, we do not use any transpositions of Bach chorales.

Then, we perform a series of generation-train updates. In each iteration, we generate $N$ chorales and score each one. We set our threshold as the score of the lowest-scoring Bach chorale, with the intuition that every real Bach chorale should be selected by our score function. We update the model by training on the selected examples for $M$ epochs.

{{< figure src="../../img/architecture.png" title="Augmentative generation training procedure applied to DeepBach" >}}

## DeepBach
We apply our method to [DeepBach]({{< relref "#deepbach_cite" >}}), an architecture that was designed for the generation of Bach chorales. It combines two recurrent networks (LSTMs) and two feedforward networks. One recurrent network sums up past information, one recurrent network sums up future information, and a non-recurrent network represents notes occurring at the same time. The outputs of the three models are merged and passed as the input of a final feedforward neural network, whose output activation function is softmax. This architecture is replicated four times, one for each voice in a chorale. Generation is done by sampling, using a pseudo-Gibbs sampling incremental and iterative algorithm.
{{< figure src="../../img/deepbach_architecture.png" title="DeepBach’s neural network architecture for prediction of the soprano voice" width="30%">}}

We use the same data representation as in the original DeepBach paper. In particular, notes are encoded as MIDI pitches and discretized into sixteenth notes. A hold symbol “--” is used to indicate whether the preceding note is held. The fermata symbol for Bach chorales is explicitly considered in the metadata to help produce structure and coherent phrases.

## Score function
The score function determines which generated chorales are used to update the model. Given a chorale, the score function should output a value that captures how much it looks like real Bach chorales. Our basic method is to compare feature distributions between a given chorale and real Bach chorales. (To be clear, a distribution is a histogram of counts, normalized by the total number of notes.) For each feature $f$, we use the [Wasserstein metric](https://en.wikipedia.org/wiki/Wasserstein_metric) to measure the distance between the distribution $P\_{c}^f$ of the given chorale $c$ and the ground-truth distribution $P\_{\text{Bach}}^f$ over real Bach chorales. The resulting value, $Wass\left(P\_{c}^f,P\_{\text{Bach}}^f\right)$, is weighted by $w^f$. The overall score is therefore a weighted sum of these distances over all features $f$. That is,
$$S( c )=\sum\_{f\in\text{features}} Wass\left(P\_{c}^f,P\_{\text{Bach}}^f\right)\cdot w^f$$
Since low distances represent high similarity between a given chorale and real Bach chorales, we subtract $S( c )$ from a large constant so that higher scores correspond to better chorales.

The weights $w^f$ are carefully handpicked in an iterative process where we modify the weights, judge its correlation with our own perception of chorale quality, and modify the weights further. For each feature we take into account the natural value range of $Wass\left(P\_{c}^f,P\_{\text{Bach}}^f\right)$, its empirical usefulness in distinguishing chorale quality, and its overall musical importance to a good chorale.

The features we use in the score function are notes, rhythm, interval, parallel errors, and other voice leading errors. We describe these next.

### Notes
The note distribution is the distribution of notes in scale degrees. The ground-truth note distribution for Bach chorales in major and Bach chorales in minor are calculated separately. They are shown below.

{{< figure src="../../img/note_distributions.png" title="Distribution of notes (in scale degrees) over Bach chorales" >}}

For an example, consider the following generated chorale $c$ and its note distribution $P\_{c}^\text{note}$.

{{< figure src="../../img/chorale_ex.png" title="The score of a chorale generated by our base model" width="70%">}}
{{< figure src="../../img/chorale_ex_distribution.png" title="Distribution of notes (in scale degrees) for an example chorale" width="50%">}}

Since this chorale is in G major, the chorale note distribution is compared to the ground-truth major note distribution to obtain $Wass\left(P\_{c}^\text{note},P\_{\text{Bach}}^{\text{note}\,(M)}\right)$.

### Rhythm
The rhythm distribution is the distribution of note lengths in units of quarter notes.

From the ground-truth rhythm distribution shown below, we see that Bach chorales are fairly saturated with quarter notes and eighth notes.

{{< figure src="../../img/rhythm_distribution.png" title="Distribution of note lengths (in units of quarter notes) over Bach chorales" width="50%">}}

### Interval
The directed interval distribution is the distribution of directed interval sizes.

Given the ground-truth interval distribution shown below, we should expect many small intervals for more singable lines, compared to leaps that are large or dissonant. For easier visualization, we include only intervals that have at least a $0.0025$ chance of happening [^1].

{{< figure src="../../img/directed_interval_distribution.png" title="Distribution of intervals over Bach chorales" width="50%">}}

### Parallel errors
The parallel errors distribution is the distribution of parallel fifths and parallel octaves errors.

{{< figure src="../../img/parallel_error_distribution.png" title="Distribution of parallel errors over Bach chorales" width="50%">}}

However, note here that what matters is not only the ratio of parallel 5ths to parallel octaves, but the absolute count of these errors relative to the total number of notes. Therefore, $w^\text{parallel}$ includes a term
$$\frac{\text{error to note ratio of chorale }c}{\text{error to note ratio of Bach}}$$

This term is large if the given chorale has a large error to note ratio compared to real Bach chorales, thereby penalizing the given chorale.

### Other errors
The other errors distribution is the distribution of hidden 5ths, hidden octaves, voice overlaps, voice crossings, and voice range violations. We separate this feature from the parallel errors because whereas we do not expect to see many parallel fifths and octaves, we do expect a large number of hidden fifths and octaves.

{{< figure src="../../img/error_distribution.png" title="Distribution of other errors over Bach chorales" width="50%">}}

Similarly to the previous feature, $w^\text{errors}$ includes a term to account for the absolute count of errors in addition to the relative probabilities in the distribution.

# Experiments
In our experiments, we first show that the score function effectively distinguishes real Bach chorales from generations. Then we carry out human evaluations to compare generations of three models: our model trained through augmentative generation, our base model, and the model in the original DeepBach paper.
## Evaluating the score function
Because the score function measures how similar a given chorale is to real Bach chorales, a simple evaluation method is to validate that real Bach chorales score higher than generated chorales. Therefore, we plot the distribution of scores for real Bach chorales and chorales generated by our base model.
{{< figure src="../../img/score_distribution.png" title="The score distribution for Bach chorales and generated chorales" width="100%">}}
These plots show that Bach chorales score much better on average than generated chorales, indicating that the score function serves its purpose of measuring similarity to real Bach chorales. We also see that generated chorales display high variation in quality, which demonstrates the importance of an external evaluation metric for generated chorales.

Note that based on our score threshold, the generated chorales which which would be selected can be visualized as those that "overlap" with real chorales in the score distribution.
## Model details
We updated the model for $43$ iterations. Each iteration, we generated $50$ training examples for consideration. We train on the selected chorales for $2$ epochs. Of note, after this process our model saw less than one-fifth of the training data used in the original DeepBach paper, which consists of Bach chorales and their transpositions.
## Evaluation
In the paired discrimination task, participants with music training are presented with pairs of audio clips where one is a real Bach chorale and one is generated output. The generated output comes from one of three models: our model trained through augmentative generation on Bach chorales and high-quality generations, our base model trained on only Bach chorales, and the model in the original DeepBach paper trained on Bach chorales and their transpositions. Participants are told that not all generated outputs are produced by the same model. We then compare human accuracy at detecting generated music for each model.

For preliminary experiments, we have $n=1$ participant with a conservatory degree in piano performance. We presented 30 pairs of audio samples, using $30$ Bach chorales and $10$ randomly selected generations from each model. Each audio sample represents $4$ measures of music, rendered with MuseScore at $80$ beats per minute.

| Model                                                         | Accuracy |
|---------------------------------------------------------------|----------|
| DeepBach (trained on Bach chorales)                           | 0.90     |
| DeepBach (trained on Bach chorales + transpositions)          | 0.90     |
| DeepBach (trained on Bach chorales + augmentative generation) | 0.90     |

Unfortunately our experiments were conducted under the grueling demands of finals week, and not yet extensive enough to be discriminative. Future work is discussed below.

# Music samples
See if you can discriminate between Bach chorales and generated chorales yourself! In each of the following pairs, one chorale is composed by Bach and one is generated. The answers are here [^2].

1. The generated chorale comes from DeepBach (trained on Bach chorales), which is our base model.
{{< audio wav="../../audio/4_real.wav" >}}
{{< audio wav="../../audio/4_fake.wav" >}}

2. The generated chorale comes from DeepBach (trained on Bach chorales + transpositions), from the original DeepBach paper.
{{< audio wav="../../audio/3_fake.wav" >}}
{{< audio wav="../../audio/3_real.wav" >}}

3. The generated chorale comes from DeepBach (trained on Bach chorales + augmentative generation), which is our contribution.
{{< audio wav="../../audio/6_real.wav" >}}
{{< audio wav="../../audio/6_fake.wav" >}}

# Upcoming work and ideas
1. Every handful of generate-train updates, do another pass through the original Bach chorale training data. This way, we ensure that the model does not "forget" what gold-standard data looks like.
2. Show (hopefully) that the score distribution of generated chorales is increasing over time and becoming more similar to the score distribution over Bach chorales. This shows that according to the score function, generations are improving as generate-train updates go on.
3. Currently, there is no method of determining when to discontinue generate-train iterations. We need to implement code to evaluate the validation loss at the end of every iteration, in order to stop when training on more generations is no longer improving model performance on validation data.
4. Conduct a much more extensive version of the human evaluation. In addition to measuring discrimination accuracy, we intend to measure decision time as an indicator of confidence. This is motivated by an observation we made in our preliminary experiment: even though our participant displayed the same performance for each model, his amount hesitation varied significantly (sometimes, he did not even listen to the full audio sample; other times, he deliberated thoughtfully and made a "guess".) We hope to show that our model causes longer decision times, suggesting that its generations are more challenging to distinguish from real Bach chorales. Moreover, we hope to show that decision time correlates positively with the chorale score: that is, the higher-scoring a chorale is, the longer it takes a trained listener to determine whether it is real or generated.

# References
1. <a name="challenges_directions_cite"> Jean-Pierre Briot and François Pachet. "Music Generation by Deep Learning – Challenges and Directions."_Neural Computing and Applications_, 2018.
2. <a name="deepbach_cite"> Gaetan Hadjeres, Francois Pachet, Frank Nielsen. “DeepBach: a Steerable Model for Bach chorales generation.” _The International Conference on Machine Learning_ (ICML), 2016.


[^1]: The interval types that were excluded are: M6, m6, d1, m-6, d-4, M-6, m7, d4, d-7, M7, d5, m-7, M9, A5, A4, M-7, M-9, A8, P11, M10, P12, A2, d-3, A-4, m10, d7, P15, A-5, and d3.
[^2]: (1) first chorale is real. (2) second chorale is real. (3) first chorale is real.
