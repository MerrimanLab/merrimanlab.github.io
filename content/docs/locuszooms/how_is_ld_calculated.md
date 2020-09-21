---
title: "How is LD calculated?"
date: 09-18-2020
draft: false
weight: 0
enableToc: true
tocLevels:
  - h2
  - h3
  - h4
---

# On-the-fly LD calculation

The Locus Zoom code calculates the LD of the region (+/-500kb) using the specified SNP (usually lead) as the reference variant to calculate the LD with other variants in the region.

This is done by calling external tools within `R`:
- `bcftools` to extract the relevant region from the 1000 Genomes reference VCF
- `plink` to calculate the LD of specified variant with all other variants in the extracted region

This is the default behaviour when you don't provide the LD information to the LZ call.

## Caveats

### Variants not present in the reference data

Though it could be useful at times, on-the-fly LD calculation could be problematic in certain situation.
For example, when the lead variant or the variant you want to plot is present in the summary statistics, but not in the 1000 Genomes reference data, the code will terminate with an error.

Possible solutions are:
- use a different reference panel for LD calculation
- use a different (surrogate) variant for the LZ plot

### Cluttering of work directory

Right now, the calculated LD data are saved in the working directory.
If you are generating many plots, this may clutter your working directory extremely quickly.

We have not yet implemented any option to turn this feature off, but a work around is to:
1. Generate all of the LD for your variants **before** running LZ plot
2. When you run the LZ code, provide the LD information so that it doesn't calculate the LD on the fly

[Here](/docs/locuszooms/multiple_lz_plots/#modify-the-loop) is an example of how you would provide LD information for generating multiple LZ plots.

