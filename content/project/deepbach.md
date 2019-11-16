---
title: Augmentative generation for Bach chorales
summary: Using heuristic evaluations of generated music to train a Bach chorale generator on high-quality generations
tags:
- research
- class project
- ongoing
date: "2019-11-01"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

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

A major limitation of music generation systems for a specific domain is finite training data. Researchers have augmented data with techniques such as transposition, which loses compositional information specific to its original key, or by extracting multiple short segments from each piece, which loses long-term coherence. We propose an alternative method which augments a dataset limited in training examples by using an existing model’s generated output weighted by a hand-crafted score function, to feed back as new training data to update the model. We try this method with DeepBach, a model trained to generate chorales in the style of Bach, a task where gold data is inherently limited to the 389 chorales Bach wrote in his lifetime. While this is a large output for a single composer, it is not enough for training deep models. Our hypothesis is that training on high-quality generated output is more useful than training on simple transformations of the same music.

This is very much a work in progress and we hope to use our class project as an opportunity to be extra creative, so please let us know if you have any thoughts or suggestions!
