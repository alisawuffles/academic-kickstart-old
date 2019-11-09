---
title: Ensemble model for audio source separation
summary: Built an ensemble model for audio source separation that can handle mixtures whose source domain is unknown, using a confidence measure to mediate among domain-specific models based on deep clustering.
tags:
- research
date: "2019-10-01"

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

Built an ensemble model for audio source separation that can handle mixtures whose source domain is unknown, using a confidence measure to mediate among domain-specific models based on deep clustering. We derived a confidence measure based on the clusterability of the embedding space which approximates the separation quality without ground-truth comparison.
