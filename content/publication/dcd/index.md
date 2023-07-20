---
title: "A Distributed Community Detection Algorithm for Large Scale Networks Under Stochastic Block Models"
authors:
- Shihao Wu
- Zhe Li
- Xuening Zhu
author_notes:
- "Equal contribution"
- "Equal contribution"
date: "2023-05-27T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2023-07-19T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: "*Computational Statistics & Data Analysis*"
publication_short: ""

abstract: Community detection for large scale networks is of great importance in modern data analysis. In this work, we develop a distributed spectral clustering algorithm to handle this task. Specifically, we distribute a certain number of pilot network nodes on the master server and the others on worker servers. A spectral clustering algorithm is first conducted on the master to select pseudo centers. Next, the indexes of the pseudo centers are broadcasted to workers to complete the distributed community detection task using an SVD (singular value decomposition) type algorithm. The proposed distributed algorithm has three advantages. First, the communication cost is low, since only the indexes of pseudo centers are communicated. Second, no further iterative algorithm is needed on workers while a “one-shot” computation suffices. Third, both the computational complexity and the storage requirements are much lower compared to using the whole adjacency matrix. We develop a Python package DCD (The Python package is provided in [https://github.com/Ikerlz/dcd](https://github.com/Ikerlz/dcd).) to implement the distributed algorithm on a Spark system and establish theoretical properties with respect to the estimation accuracy and mis-clustering rates under the stochastic block model. Experiments on a variety of synthetic and empirical datasets are carried out to further illustrate the advantages of the methodology.

# Summary. An optional shortened abstract.
# summary: 

tags:
# - Source Themes
featured: false

# links:
# - name: ""
#   url: ""
url_pdf: https://www.sciencedirect.com/science/article/abs/pii/S0167947323001056
url_code: 'https://github.com/Ikerlz/dcd'
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
# image:
#   caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/jdD8gXaTZsc)'
#   focal_point: ""
#   preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
# slides: example
---

<!-- {{% callout note %}}
Click the *Cite* button above to demo the feature to enable visitors to import publication metadata into their reference management software.
{{% /callout %}}

{{% callout note %}}
Create your slides in Markdown - click the *Slides* button to check out the example.
{{% /callout %}}

Supplementary notes can be added here, including [code, math, and images](https://wowchemy.com/docs/writing-markdown-latex/). -->
