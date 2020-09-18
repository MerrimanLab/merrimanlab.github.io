---
title: "Locus Zoom"
date: 2020-01-28T00:36:39+09:00
draft: false
weight: 2
collapsible: true
enableToc: true
tocLevels:
  - h2
  - h3
  - h4
---

## Description

This is the documentation for the _in-house_ Locus Zoom software, developed by Tanya, Matt, and Riku.

If you only need to generate a few plots, you may want to take a look at [locuszoom.org](http://locuszoom.org/) instead.

## Requirements and prerequisites

### Software

You will need the following software to run this tool:
- Latest version of R
- Locus Zoom code from [GitHub](https://github.com/Geeketics/LocusZooms)
- `bcftools`
- `plink` version 1.90 or later

### Data

You will need the following data to run this tool:
- Summary level data with the following information:
  - Chromosome
  - Position
  - rsID
  - Measure of significance (P or BF)
- 1000 Genomes Project Phase 3 reference data
- UCSC gene region information

Optionally, you may provide the following:
- Output from MAGMA (for colouring the genes based on MAGMA significance)

## Table of Content

