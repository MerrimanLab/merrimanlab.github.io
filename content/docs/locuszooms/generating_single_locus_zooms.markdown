---
title: "How to generate LZ plot"
date: 09-18-2020
draft: false
weight: 2
output:
  md_document:
    variant: markdown_github
    preserve_yaml: true
    pandoc_args: ["--wrap=none"]
---

# Make Locus Zoom-like plots with your own LD matrix

This script creates an R function to create regional Manhattan plots with points coloured according to LD and genes annotated beneath.

## Input file format

You will need at least two files to generate a Locus Zoom (LZ) plot: GWAS summary stats file, and a "gene" file.

<!-- TODO: need to link "access" to a quickref that points to where the data is -->
The LD file can be generated [on the fly](/docs/locuszooms/how_is_ld_calculated), as long as you have access to the 1000GP reference data and the correct tools to calculate the LD (see [Before you begin](/docs/locuszooms/before_you_begin)).

Please also note that **the column names of the required files must match exactly to those specified below**, so please take your time to check your headers and edit it to match with the specification.

### GWAS summary statistics

Though you may have more information in your GWAS summary statistics, you only need the following four columns:

| Column name   | Description                                    |
| :------------ | ---------------------------------------------- |
| `CHR`         | Chromosome number of the variant               |
| `SNP`         | rsID of the variant                            |
| `BP`          | Base position of the variant                   |
| `P`           | P-value of the variant                         |

Instead of using `P` as significance, you may use `logBF` (e.g. from a trans-ancestry meta-analysis that used MANTRA).

### Gene file

A file of the genes within the region for use in the annotation step.

This file must have four columns:

| Column name   | Description                                    |
| :------------ | ---------------------------------------------- |
| `Gene`        | Gene name                                      |
| `Chrom`       | Chromosome number of the gene                  |
| `Start`       | Start position of the gene                     |
| `End`         | End position of the gene                       |

The `UCSC_GRCh37_Genes_UniqueList.txt` file is provided with the [code](https://github.com/Geeketics/LocusZooms).

### LD file


This is a file that contains the LD between the SNP to be labelled (top-hit / SNP of interest) and the SNPs included in the GWAS summary statistics.

This file must contain the following two columns:

| Column name   | Description                                                     |
| :------------ | ----------------------------------------------                  |
| `SNP_B`       | List of all the SNPs in the region of interest                  |
| `R2`          | The r<sup>2</sup> LD-value of each SNP with the SNP of interest |

The SNP names of the LD file **must** match the SNP names in the GWAS summary statistics file.
Therefore, we recommend you to use rsID wherever possible to match with the 1000GP reference data (used to calculate the LD).

To calculate the LD yourself, follow the code below:


```bash
bcftools view -r CHR:START-END /path/to/reference/chr/vcf.gz -Oz -o /path/to/output/tmp.vcf.gz
plink1.9 --vcf /path/to/output/tmp.vcf.gz --allow-no-sex --snps-only --r2 inter-chr --ld-snp <rsid-of-interest> --ld-window-r2 0 --out /path/to/output/ld_file
```

where:

| Column name        | Description                                    |
| :------------      | ---------------------------------------------- |
| `CHR`              | Chromosome number                              |
| `START`            | Start position of the region of interest       |
| `END`              | End position of the region of interest         |
| `rsid-of-interest` | rsID of interest (usually the lead variant)    |

For more information on why this could be useful, take a look at [Generate multiple LZ plots](/docs/locuszooms/multiple_lz_plots/#ld-calculation----on-the-fly-or-pre-calculated).

## Examples

### Simple Locus Zoom

You will be able to run the following code and produce the same Locus Zoom plot using the example data set from the [GitHub repo](https://github.com/Geeketics/LocusZooms).


```r
# Load necessary files into R
Example.assoc.linear <- read.delim("Example.assoc.linear", stringsAsFactors = FALSE, header = TRUE)
Example.ld <- read.table("Example.ld", stringsAsFactors = FALSE, header = TRUE)
UCSC_GRCh37_Genes_UniqueList.txt <- read.delim("Example.genes", stringsAsFactors = FALSE, header = TRUE)

# Load the locuszoom function into R
source("functions/locus_zoom.R")

# Create a LocusZoom-like plot
locus.zoom(data = Example.assoc.linear,
           region = c(16, 53340000, 54550000),
           offset_bp = 0,
           ld.file = Example.ld,
           genes.data = UCSC_GRCh37_Genes_UniqueList.txt,
           plot.title = "Association of FTO with BMI in Europeans",
           file.name = "Example.jpg",
           secondary.snp = c("rs1121980", "rs8060235"),
           secondary.label = TRUE)
```

![](docs/locuszooms/Example.jpg)

### Locus Zoom with coloured genes based on MAGMA output

_This is not reproducible from the example data._


```r
locus.zoom(data = EUR_meta_full1_clean_rsid.nfiltered_chr7,
           gene = "MLXIPL",
           offset_bp = 500000,
           genes.data = UCSC_GRCh37_Genes_UniqueList,
           plot.title = "Association of MLXIPL with gout in Europeans",
           file.name = "alternateExample.jpg",
           genes.pvalue = European_GWAS_2020_test.genes_named,
           colour.genes = TRUE)
```

![](docs/locuszooms/alternateExample.jpg)

