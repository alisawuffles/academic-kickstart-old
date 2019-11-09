---
title: "Multi-sense Definition Modeling using Word Sense Decompositions"
authors:
- Ruimin Zhu
- Thanapon Noraset
- admin
- Wenxin Jiang
- Doug Downey

date: "2019-09-01"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2017-01-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["3"]

# Publication name and optional abbreviated publication name.
publication: Manuscript
publication_short: manuscript

abstract: Word embeddings capture syntactic and semantic information about words. Definition modeling aims to make the semantic content in each embedding explicit, by outputting a natural language definition based on the embedding. However, existing definition models are limited in their ability to generate accurate definitions for different senses of the same word. In this paper, we introduce a new method that enables definition modeling for multiple senses. We show how a Gumble-Softmax approach outperforms baselines at matching sense-specific embeddings to definitions during training. In experiments, our multi-sense definition model improves recall over a state-of-the-art single-sense definition model by a factor of three, without harming precision.

# Summary. An optional shortened abstract.
summary: We introduce a method of definition modeling for multiple senses of a word.

tags:
- definition modeling
featured: true

links:
- name: ArXiv
  url: https://arxiv.org/pdf/1909.09483
url_pdf: https://arxiv.org/pdf/1909.09483.pdf
url_code: 'https://github.com/lrkmr/w2mdef'
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
projects:
- multidef

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: example
---
