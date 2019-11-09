---
title: Analysis of error types in multi-sense definition generation
summary: Evaluated the settings under which a multi-sense definition modeling system succeeded and failed
tags:
- research
date: "2019-06-01"

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

Evaluated the settings under which a multi-sense definition modeling system succeeded and failed by investigating whether certain attributes of words and atoms were predictive of model performance. Used logistic regression with these attributes (e.g. word frequency, embedding norm, part of speech) to predict individual error types as well as a manual evaluation score. We found that atom weight was a significant predictor for score, showing that atoms with greater weights are likely to represent more dominant sense that are easier to define.
