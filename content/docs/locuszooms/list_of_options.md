---
title: "List of options"
date: 09-18-2020
draft: false
weight: 0
enableToc: true
tocLevels:
  - h2
  - h3
  - h4
---

## List of options

### Compulsory flags:

One of `snp`, `gene`, or `region` must be specified to create the plot:

Options      | Description                                                                                                                                                                                              | Default value
--------     | ------------                                                                                                                                                                                             | -------
`snp`        | specify the SNP to be annotated (you must also include `ignore.lead = TRUE` if choosing this option)                                                                                                     | NA
`gene`       | specify the Gene to make the plot around                                                                                                                                                                 | NA
`region`     | specify the chromsome region you want to plot (must be specified as `c(chr, start, end)`                                                                                                                 | NA
`data`       | specify the data.frame (or a list of data.frames) to be used in the plot (requires the columns "CHR", "BP", "SNP", and either "P" or "logBF")                                                            | NULL
`genes.data` | specify a data.frame with gene locations to plot beneath the graph (requires the columns "Gene", "Chrom", "Start", and "End") - the `UCSC_GRCh37_Genes_UniqueList.txt` in this repo can be used for this | NA
`plot.title` | specify a title to go above your plot                                                                                                                                                                    | NA
`file.name`  | specify a filename for your plot to be saved to                                                                                                                                                          | NA

### Optional flags:

Options           | Description                                                                                                                                                            | Default value
--------          | ------------                                                                                                                                                           | ------------
`ld.file`         | specify a data.frame with LD values relevant to the SNP specified by `snp` (requires the columns "SNP_B" and "R2")                                                     | NULL
`offset_bp`       | specify how far either side of the `snp`, `gene`, or `region` you want the plot to extend                                                          | 200000
`noncoding`       | when using the UCSC gene list you can specify whether you want to plot the non-coding genes                                                         | FALSE
`plot.type`       | specify the file format of the plot (options are "jpg" or "svg")                                                                                    | "jpg"
`nominal`         | specify the nominal significance level to draw on the plot (in -log10(_P_)
`significant`     | specify the significance level to draw on the plot (in -log10(_P_)
`secondary.snp`   | provide the list of secondary SNP IDs (must match IDs in results file) to be highlighted on the plot                                                                   | NA
`secondary.label` | specify whether to label the secondary SNPs on the plot                                                                                             | FALSE
`genes.pvalue`    | specify a data.frame of p-values (e.g. MAGMA results) associated with each gene (requires the columns "Gene" and "P")                                                  | NULL
`colour.genes`    | specify whether to colour genes based on a p-value provided in gene.pvalue                                                                          | FALSE
`population`      | specify the 1000 genomes population to use when calculating LD if ld.file = NULL (options are "AFR", "AMR", "EAS", "EUR", "SAS", "TAMA", and "ALL") | "EUR"
`sig.type`        | specify whether the y-axis should be labelled as -log10(_P_) or log10(BF) (options are "P" or "BF")                                                   | "P"
`nplots`          | specify whether multiple results plots will be saved into your jpeg file (e.g. plot two GWAS results one above another)                             | FALSE
`ignore.lead`     | specify whether to ignore the SNP with the smallest P and use the SNP specified by 'snp' to centre the plot                                         | FALSE
`rsid.check`      | specify whether to check if the SNPs are labelled with rsIDs - should only matter if script is calculating LD for you                                | TRUE

