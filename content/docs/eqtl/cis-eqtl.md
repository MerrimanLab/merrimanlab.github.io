---
title: cis-eQTL
author: Murray Cadzow
date: '2020-09-14'
slug: cis-eqtl
categories: [eqtl]
tags: []
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
---

Repo: https://github.com/MerrimanLab/QTL_pipelines

This pipeline runs a co-localisation analysis using a list of key SNPs, a GWAS results file, and the GTEx dataset.

```
bash cis_eqtl_pipeline.sh my_gwas.txt my_snplist.txt 1000000
```