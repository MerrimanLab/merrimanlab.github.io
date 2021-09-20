---
title: "Adding Annotations to VCFs"
date: 2021-09-21T11:29:24+13:00
author: "Murray Cadzow"
draft: false
weight: 0
enableToc: true
tags: ["howto","vcf"]
tocLevels: ["h2", "h3", "h4"]
---


`example.vcf.gz` is a VCF file of three human subjects aligned to GRCh37 and varaint called following the GATK best practices that had been annotated with rsIDs from dbSNP v151 and further annotated using dbNSFP4.0a and snpEff so includes annotations such as:

- Allele Frequencies for variants from public databases 1000 Genomes, ExACm gnomad, etc
- functional annotations: missense, intron, 3'/5' UTR, etc
- gene names 

In order to create it refer to the [adding VCF annotations howto](2021-09-vcf-annotation)

The main annotations that are going to be used are:

```
##INFO=<ID=ANN,Number=.,Type=String,Description="Functional annotations: 'Allele | Annotation | Annotation_Impact | Gene_Name | Gene_ID | Feature_Type | Feature_ID | Transcript_BioType | Rank | HGVS.c | HGVS.p | cDNA.pos / cDNA.length | CDS.pos / CDS.length | AA.pos / AA.length | Distance | ERRORS / WARNINGS / INFO'">
##INFO=<ID=LOF,Number=.,Type=String,Description="Predicted loss of function effects for this variant. Format: 'Gene_Name | Gene_ID | Number_of_transcripts_in_gene | Percent_of_transcripts_affected'">

##INFO=<ID=dbNSFP_ExAC_NFE_AC,Number=A,Type=String,Description="Field 'ExAC_NFE_AC' from dbNSFP">
##INFO=<ID=dbNSFP_gnomAD_exomes_NFE_AF,Number=A,Type=String,Description="Field 'gnomAD_exomes_NFE_AF' from dbNSFP">
##INFO=<ID=dbNSFP_gnomAD_exomes_controls_NFE_AF,Number=A,Type=String,Description="Field 'gnomAD_exomes_controls_NFE_AF' from dbNSFP">

##INFO=<ID=dbNSFP_gnomAD_genomes_POPMAX_AC,Number=A,Type=String,Description="Field 'gnomAD_genomes_POPMAX_AC' from dbNSFP">
##INFO=<ID=dbNSFP_gnomAD_genomes_POPMAX_AN,Number=A,Type=String,Description="Field 'gnomAD_genomes_POPMAX_AN' from dbNSFP">
##INFO=<ID=dbNSFP_gnomAD_genomes_POPMAX_AF,Number=A,Type=String,Description="Field 'gnomAD_genomes_POPMAX_AF' from dbNSFP">

```

All of these annoatations are in the INFO column of the VCF. The 3 dnNSFP AF annotations are population allele frequencies that will help identify rare variants, along with the POPMAX annotations explained below. 

```
gnomAD_genomes_POPMAX_AC: Allele count in the population with the maximum AF
gnomAD_genomes_POPMAX_AN: Total number of alleles in the population with the maximum AF
gnomAD_genomes_POPMAX_AF: Maximum allele frequency across populations (excluding samples of Ashkenazi, Finnish, and indeterminate ancestry)
```

## snpSift

[Documentation](http://snpeff.sourceforge.net/SnpSift.html#intro)

snpSift comes with snpEff and is used for filtering and annotating. 


Ordinarily snpSift would be run like so:
```
java -jar SnpSift.jar <arguments>
```

**On the server however it is run like this:**
```
module load snpEff/snpEff_4.3t # only needs to be run once after login
java -jar $SNPSIFT <arguments>
```

I have used the server method in the examples below.

### Filter

[Filter Documentation](http://snpeff.sourceforge.net/SnpSift.html#filter)

It is worth taking a look at the actual documentation as it explains how to create filters and how the syntax works and provides some good examples.

Especially look under the heading "SnpEff 'ANN' fields".

I have pulled out and adapted a couple that are of immediate interest.


### Filter for missense variants

To grab all lines that had "missense_variant"
```
java -jar $SNPSIFT filter "(ANN[*].EFFECT has 'missense_variant')" example.vcf.gz | bgzip -c > missense.vcf.gz
```


Originally 9478 variants, after filtering now have 88

To get this number I use the following, which removes the header lines and counts the number of lines
```
zgrep -v '^#' missense.vcf.gz | wc -l
```

The ANN[\*] means look through all of the sets of `ANN` annotations for a variant (most variants will have multiple sets of within the `ANN`, usually one per transcript). If you were after the first you would use `ANN[0]` (because it is zero indexed).

The use of _has_ means that all effect types will be selected so long as it included _missense_variant_, such as "missense_variant&splice_region_variant" . If you want _ONLY_ "missense_variant" then use `ANN[*] = 'missense_variant'`

### Filter for population AFs

```
java -jar $SNPSIFT filter "(dbNSFP_gnomAD_genomes_POPMAX_AF < 0.05 )" example.vcf.gz | bgzip -c > gnomAD_genomes_POPMAX_AF_lt0.05.vcf.gz
```
Originally had 9478 variants, now have 50

> N.B. depending on the annotation you might need to add an explicit filter to remove variants with an AF of 1. This could be done by, for example adding 
> ```
> & !(dbNSFP_gnomAD_genomes_POPMAX_AF > 0.99)
> ```
> 
>  (the dbNSFP sometimes records allele frequency fields with an AF of 1 as 1.00000e+00 which seems to cause issues with snpSift)


When it comes to filtering it can useful to remove all the additional annotations so you can focus on those being used in the filter

For instance using `bcftools` you can specify which columns and sub-columns you want to keep. In the example below it will remove all annotation in the INFO column but keep the 3 sub-columns of _AF_, _dbNSFP_gnomAD_genomes_POPMAX_AF_ and _INFO/dbNSFP_gnomAD_exomes_POPMAX_AF_.
```
bcftools annotate -x ^INFO/AF,INFO/dbNSFP_gnomAD_genomes_POPMAX_AF,INFO/dbNSFP_gnomAD_exomes_POPMAX_AF example.vcf.gz
```

So in terms of a testing pipeline you could do the following:
```
java -jar $SNPSIFT filter "(dbNSFP_gnomAD_genomes_POPMAX_AF < 0.05 )" example.vcf.gz | bcftools annotate -x ^INFO/AF,INFO/dbNSFP_gnomAD_genomes_POPMAX_AF,INFO/dbNSFP_gnomAD_exomes_POPMAX_AF | bgzip -c > gnomAD_genomes_POPMAX_AF_lt0.05.vcf.gz
```

### Multiple filters

We can combine multiple filters such as the variant effect type and a population allele frequency threshold. e.g. if we combine the previous two filters

```
java -jar $SNPSIFT filter "(ANN[*].EFFECT has 'missense_variant' & (dbNSFP_gnomAD_genomes_POPMAX_AF < 0.05 ))" example.vcf.gz | bgzip -c > missense_gnomAD_genomes_POPMAX_AF_lt0.05.vcf.gz
```
 Originally 9478 variants, now 48