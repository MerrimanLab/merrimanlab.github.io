---
title: "Before You Begin"
date: 2020-09-18T00:36:39+09:00
draft: false
weight: -1
enableToc: true
tocLevels:
  - h2
  - h3
  - h4
---

## Requirements and prerequisites

Here is a list of requirements that you will need in order to run this tool (relatively) smoothly.
If you have access to the Department of Biochemistry server, you should have little to no problems to meet these requrirements.

### Software

You will need the following software to run this tool:
- Latest version of `R`
- Locus Zoom code from [GitHub](https://github.com/Geeketics/LocusZooms)
- `bcftools`
- `plink` version 1.90 or later

### Data

You will need the following data to run this tool:
- 1000 Genomes Project Phase 3 reference data
- UCSC gene region information
- Summary level data with the following information:
    - Chromosome
    - Position
    - rsID
    - Measure of significance (P or BF)

### Optional data

Optionally, you may provide the following:
- Output from MAGMA (for colouring the genes based on MAGMA significance)


