---
title: Analysis of error types in multi-sense definition generation
summary: Evaluated the settings under which a multi-sense definition modeling system succeeded and failed
tags:
- research
- nlp
date: "2018-12-01"

# Optional external URL for project (replaces project detail page).
# external_link: ""

image:
  caption:
  focal_point:
  preview_only: True

# links:
# - icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
url_code: "https://github.com/alisawuffles/multidef"
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

Word embeddings are vector representations of words that form the foundation for many NLP tasks. While they have been shown to capture semantic similarities and encode analogical relations between words, these comparison tasks only evaluate an embedding’s information indirectly. In contrast, the task of generating natural language definitions from word embeddings provides a more transparent view of the information captured in a word embedding.

However, existing definition models are limited in their ability to generate accurate definitions for different senses of the same word. In this paper, we introduce a new method that enables definition modeling for multiple senses. Some selected examples are shown below.

{{< figure src="../../img/multidef_ex.png" title="Selected examples of the multi-sense definition model's outputs" width="50%">}}

## Quantitative analysis of error types
In my project, my goal was to better understand the settings under which our best model (W2MDEF-GS) succeeds and fails.

To evaluate the generated definitions, we manually label outputs as one of four categories: ${\bf I}$ the output is correct; ${\bf II}$ the output has _either_ a syntax/fluency error or a semantic issue, but not both; ${\bf III}$ the output has both a syntactic and semantic error but is not completely wrong; and ${\bf IV}$ where the output is completely wrong. When evaluating precision and recall, the four labeling categories are given scores $1.0$, $0.6$, $0.3$, and $0.0$ respectively.  A breakdown of the error types is shown below.

{{< figure src="../../img/multidef_error_types.png" title="A breakdown of error types, and the percentage of each in our best model vs. the baseline. For each error type, one example is provided. An output can have more than one error type." width="50%">}}

I investigated whether certain attributes of words and atoms are predictive of model performance. The word attributes we considered included the ranking of word frequency, the number of ground-truth definitions of the word, the semantic diversity of ground-truth definitions, and the word embedding norm.  Atom attributes included the atom weight after decomposition and the part of speech of the atom. We used logistic regression with these attributes to predict two different output variables: individual error types and the score from $0$ to $1$. For predicting the score, we trained logistic regression to minimize the cross-entropy between the model output and the score (i.e., we treated the non-0/1 score labels as probabilities). We performed 5-fold validation, where atoms belonging to the same word must always be in the same fold.

I was unable to predict the individual error labels with accuracy above baseline, which suggests the attributes were not good predictors given the scale of data we had available, and demonstrates that definition generation is still a challenging problem. However, the score prediction model predicts the score with $0.48$ loss, compared to the $0.53$ baseline, using only atom weight as an attribute, which is a significant predictor with p-value $< 0.01$. We hypothesize that this is because atoms with greater weight are more likely to represent more dominant senses that are easier to define.  In fact, the atoms with the top 10\% in weight have an average score of $0.35$, substantially higher than the average score of $0.19$ across all atoms.

In a separate analysis of part of speech, I found that adjectives achieve the highest average score. This is likely the result of having the shortest average output definition length, making the redundancy label (the most common syntactical failure mode) rare. (Adjectives have the redundancy label $3\%$ of the time, compared to $15\%$ for nouns.)

|POS|average score|average length|
|---|---|---|
|adj|0.39|5.7|
|noun|0.30|10.1|
|adv|0.32|7.2|
|verb|0.31|5.9|

This work in definition modeling for single words led me to my next project, where I generate definitions for [word compounds]({{< relref "noun-compounds/index.md" >}})!
