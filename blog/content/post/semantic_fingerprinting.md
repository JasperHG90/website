---
title: "'Semantic fingerprinting' 10-K business descriptions"
layout: post
date: "2016-07-09T15:26:00"
categories: ["analysis"]
tags: ["Text analysis", "SEC", "Semantic fingerprinting", "10-K", "Annual report"]
blog: true
author: Jasper Ginn
description: "Applying Cortical's semantic fingerprinting technique to derive industry classifications"
---

For me, one of the most interesting areas of data analytics is in processing text as data. Because texts are created by humans, they tend to be difficult to interpret, biased, error-prone and idiosyncratic. This is exactly what makes text interesting as a source of data. In the past few months, I've been working on a project that involves analyzing annual reports of U.S. firms. After scraping the 10-K reports from the SEC website (using [this](https://github.com/JasperHG90/TenK) R package), we extract the business description section of each report. This section is interesting because firms are legally obligated to give an accurate overview of their business. We then use a method called [semantic fingerprinting](http://www.cortical.io/technology_semantic.html) to create a unique vector-based 'fingerprint' of the business description section. You read more about semantic fingerprinting in Cortical's [online documentation](http://www.cortical.io/resources_apidocumentation.html).

In the course of testing the results, I applied [hierarchical clustering](https://en.wikipedia.org/wiki/Hierarchical_clustering) on several samples of about 100 observations. The figure below shows the result of one of these clusters. The top-ten keywords are shown above each cluster together with their count (between parentheses).

<div>
  <img src="img/blog/semantic_fingerprinting/clusterFP.png" style="max-width:100%;">
</div>

The interesting thing is how much sense these clusters make, even though a lot of these descriptions vary a lot in length and, more crucially, by the words they use. Using techniques such as semantic fingerprinting, we can go past simple text analyses and move on to some meaningful level of 'meaning'.

I will expand on semantic fingerprinting a bit more in an upcoming post.
