---
layout: post
title: One paper accepted to the ICML 2025!
date: 2025-05-01 18:54:00-0400
inline: false
---

One paper accepted to the ICML 2025! :tada:

Reach out to corresponding authors, [Suchith Prabhu](https://suchith720.github.io/) or [Bhavyajeet Singh](https://bhavyajeet.github.io) to know more! Here's a summary of the work:

---

In a text classification task, given a query, we aim to predict the labels relevant to that query. In many cases, additional information about the query or the label is available (called metadata or memory) and can be leveraged to make better predictions. Currently popular methods, such as RAG, which retrieve this metadata, and augment it with the model input for classification or generation, tend to have high latency and are sensitive to noise.
We propose a two-stage technique to utilize this extra information while also meeting the latency constraints. First, we train a powerful oracle model that takes advantage of the metadata assuming a best-case scenario where this information is also available during inference. Then, we train an off-the-shelf classifier model as a disciple that learns to mimic the behaviour of the oracle, but under a more challenging scenario, wherein the metadata is not know apriori. In this way, we get the best of both worlds ? A model that is both efficient and fast, while benefiting from the improved accuracy gained by learning from the powerful oracle model.
We observe that this training technique (which we call Metadata-infused Oracle Guidance for Improving Classification, or MOGIC) improves overall accuracy of any existing classifier, while also offering a novel approach to incorporating extra information into the classification settings.
