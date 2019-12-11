---
title: Augmentative generation for Bach chorales
summary: Using heuristic evaluations of generated music to train a Bach chorale generator on high-quality generations
tags:
- research
- class project
- ongoing
- music
date: "2019-11-01"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption:
  focal_point:
  preview_only: True

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

A major limitation of music generation systems for a specific domain is finite training data. Consider generating Bach chorales, Chopin nocturnes, or Beatles hits. Training data in this case is limited to the compositions during the real composer's lifetime. Researchers commonly approach this problem by artificially augmenting the dataset with synthetic data, a technique known as _dataset augmentation_. In the musical domain, a natural and easy way is by transposing examples in all keys, which loses information specific to the original key. Another technique is to extract many short segments from each piece, which loses long-term coherence.

We propose an alternative method for data augmentation, which we call _augmentative generation_. Essentially we generate music, filter it by a hand-crafted score function, and feed it back as new training data to update the model. We try this method with DeepBach, a model trained to generate chorales in the style of Bach, a task where gold data is inherently limited to the 389 chorales Bach wrote in his lifetime. Our hypothesis is that training on high-quality generated output is more useful than training on simple transformations of the same music.

## Proposed method
### DeepBach
### Score function
#### Notes
#### Rhythm
#### Interval
#### Parallel errors
#### Other errors

## Experiments
### Model details
### Data

## Results
