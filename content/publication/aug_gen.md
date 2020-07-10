---
title: "Incorporating Music Knowledge in Continual Dataset Augmentation for Music Generation"
authors:
- admin
- Alexander Fang
- Gaëtan Hadjeres
- Prem Seetharaman
- Bryan Pardo
date: "2020-07-18"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2017-01-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In ICML 2020 Machine Learning for Media Discovery (ML4MD) Workshop
publication_short: In *ICML* 2020 workshop

abstract: Deep learning has rapidly become the state-of-the-art approach for music generation. However, training a deep model typically requires a large training set, which is often not available for specific musical styles. In this paper, we present augmentative generation (Aug-Gen), a method of dataset augmentation for any music generation system trained on a resource-constrained domain. The key intuition of this method is that the training data for a generative system can be augmented by examples the system produces during the course of training, provided these examples are of sufficiently high quality and variety. We apply Aug-Gen to Transformer-based chorale generation in the style of J.S. Bach, and show that this allows for longer training and results in better generative output.

# Summary. An optional shortened abstract.
summary: A generative data augmentation method for music generation systems on a resource-constrained domain

tags:
- music generation
- data augmentation
- deep learning
- domain knowledge
featured: true

links:
url_pdf: https://arxiv.org/pdf/2006.13331.pdf
url_code: 'https://github.com/asdfang/constraint-transformer-bach'
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
