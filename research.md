---
title: Research
permalink: /research/
description: Publications and projects.
---

<!--
Keep this page plain: a list of things, each with a link.

Two rules for the comments below; breaking either one silently mangles the page.

  1. A comment needs a blank line above AND below it. Without one, kramdown folds
     it into the neighbouring list, prints the delimiters as visible text, and
     nests whatever comes next inside that list.

  2. Comments cannot nest, and cannot contain a comment delimiter in their text.
     The first closing delimiter ends the comment; everything after it is back on
     the page.
-->

<!--
## Publications

- **Nothing yet**
-->

<!--
Format for a real entry:

- **[Paper title]**, with [Coauthors]. *[Venue]*, [year]. [[pdf]](#) · [[arXiv]](#)
-->

## Notes

- **Random Matrix Theory Notes:** Course notes for a comprehensive introduction to random matrix theory under Vadim Gorin. Topics include semicircle law, point processes, Tracy--Widom law, free probability, and universality. Available upon request.

- **Infinite Iterated Function Systems:** Limit sets and attractors of IIFS. Work done under Ariel Rapaport. [[pdf]]({{ '/assets/files/IIFS.pdf' | relative_url }})

## Projects
A variety of projects in machine learning and computational biology.Devised and ran a novel experiment chaining explainer models to explain each other, demonstrating that "explaining ability" converges to a stable fixed point after a few iterations — an original extension beyond the source paper

- **[Model Introspection](https://github.com/oakleafwarrior/introspection_replication)** I replicated results from this [paper](https://arxiv.org/pdf/2511.08579). I LoRA poost trained Qwen3 family of models to explain their responses on input ablation and activation patching experiments. High-order explainer models (those trained to explain previous explainer models), stabilize on evaluation metrics, but are worse at explaining the original model. Explainer models trained from smaller/quantized base models perform worse than those postrained from the target model on input ablation explanations, and perform similarly well on activation patching explanations in a small training run regime. Experiment yourself on the notebooks: [![Small Explainer Replication](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/oakleafwarrior/introspection_replication/blob/main/replication.ipynb), [![Iteration Experiment](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/oakleafwarrior/introspection_replication/blob/main/iteration.ipynb).

- **Tumor Vasculature:** In a previous life, I worked in computational immunology, using image processing tools to on spatial transcriptomics data to identify vasculature structure under Allon Wagner. It turns out this is quite hard. Contact me if you are interested in why.

- **[Numpy VAE](https://github.com/oakleafwarrior/numpy_VAE):**  I built a classic MLP encoder/decoder VAE architecture directly in NumPy using no neural network libraries. Clone the repo to run it yourself.

<!--
To hang links off an entry, put them at the end of its line:
  [[code]](https://github.com/...) · [[write-up]]({{ '/blog/' | relative_url }})
-->
