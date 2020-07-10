---
title: "Model Selection for Deep Audio Source Separation via Clustering Analysis"
authors:
- admin
- Prem Seetharaman
- Bryan Pardo
date: "2019-10-01"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2017-01-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: Submitted to the IEEE International Workshop on Machine Learning for Signal Processing (MLSP)
# publication_short: Under review

abstract: Audio source separation is the process of separating a mixture (e.g. a pop band recording) into isolated sounds from individual sources (e.g. just the lead vocals). Deep learning models are the state-of-the-art in source separation, given that the mixture to be separated is similar to the mixtures the deep model was trained on. This requires the end user to know enough about each model's training to select the correct model for a given audio mixture. In this work, we automate selection of the appropriate model for an audio mixture. We present a confidence measure that does not require ground truth to estimate separation quality, given a deep model and audio mixture. We use this confidence measure to automatically select the model output with the best predicted separation quality. We compare our confidence-based ensemble approach to using individual models with no selection, to an oracle that always selects the best model and to a random model selector. Results show our confidence-based ensemble significantly outperforms the random ensemble over general mixtures and approaches oracle performance for music mixtures.

# Summary. An optional shortened abstract.
summary: Ensemble model for audio source separation, using a confidence measure to mediate among domain-specific models

tags:
- source separation
- deep learning
- performance prediction
- ensemble methods
- deep clustering
featured: true

links:
url_pdf: https://arxiv.org/pdf/1910.12626.pdf
# url_code: 'https://github.com/pseeth/thesis_experiments/tree/confidence'
# url_dataset: '#'
# url_poster: '#'
# url_project: ''
# url_slides: ''
# url_source: '#'
# url_video: '#'

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/pLCdAaMFLTE)'
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: example
---
