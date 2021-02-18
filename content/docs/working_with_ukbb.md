---
title: Working with UK Biobank
author: Murray Cadzow
date: '2021-02-15'
slug: ukbiobank-genomic-data
categories: ["UKBB"]
tags: ["UK Biobank"]
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
---

## Load modules for processing VCFs

These programs are usually all needed in order to do analysis with the UK Biobank genetic data.
```
module load bgenix/bgenix-1.1.8
module load bcftools/bcftools-1.11
```

The format for most analysis will follow this conversion:

bgen -> vcf -> bed/bim/fam (plink)

## Extract and convert from bgen to VCF

### Extract region or SNP

This command will extract out a region from the bgen file and turn it into a vcf
```
bgenix -g ukb_imp_chr${CHR}_v3.bgen \
  -i ukb_imp_chr${CHR}_v3.bgen.bgi \
  -vcf -incl-range ${POS} | bgzip -c > new_file.vcf.gz
```

### Add on correct IDs

BUT the output isn't going to be of any use with the phenotypic data because the ids mean nothing, so we need to add on the correct ids

```
bcftools reheader -h bgen_to_vcf/new_header.txt \
  -O z \
  -o new_file.reheadered.vcf.gz new_file.vcf.gz
```

if dealing with chrX/chrXY use

```
# chrX
bcftools reheader -h bgen_to_vcf/new_header_chrX.txt \
  -O z \
  -o new_file.reheadered.vcf.gz new_file.vcf.gz

# chrXY
bcftools reheader -h bgen_to_vcf/new_header_chrX.txt \
  -O z \
  -o new_file.reheadered.vcf.gz new_file.vcf.gz
```

### Rename Contigs

The UK Biobank has the chromosomes 1-9 as 01-09 so in order to use them with other data we need to rename the contigs

```
bcftools annotate --rename-chrs bgen_to_vcf/rename_contigs.txt \
  -o new_file.reheadered.renamed.vcf.gz \
  -O z new_file.reheadered.vcf.gz
```

## Bgen to VCF in One Step

Remember if dealing with X or XY to switch the following commands to use the appropriate new header.

### Pull out range
```
bgenix -g ukb_imp_chr${CHR}_v3.bgen \
  -i ukb_imp_chr${CHR}_v3.bgen.bgi \
  -vcf -incl-range ${POS} | \
bcftools reheader \
  -h bgen_to_vcf/new_header.txt | \
bcftools annotate \
  --rename-chrs bgen_to_vcf/rename_contigs.txt | \
bgzip -c > new_file.vcf.gz && tabix -p vcf new_file.vcf.gz
```



### Pull out a specific marker

```
bgenix -g ukb_imp_chr${CHR}_v3.bgen \
  -i ukb_imp_chr${CHR}_v3.bgen.bgi \
  -vcf -incl-rsids ${RSID} | \
bcftools reheader \
  -h bgen_to_vcf/new_header.txt | \
bcftools annotate \
  --rename-chrs bgen_to_vcf/rename_contigs.txt | \
bgzip -c > new_file.vcf.gz && tabix -p vcf new_file.vcf.gz
```


## Using UKBB data in plink

Refer to the [plink manual](https://www.cog-genomics.org/plink/1.9/) for extra options or information on how to actually run plink.

```
# Current as at Feb 2021
module load plink/plink1.9b6.21
```

Convert from vcf to bed/bim/fam.
```
plink --vcf new_file.vcf.gz --make-bed --out new_file
```

At this stage it can also be useful to kick out extra samples, which can be done as part of the file conversion above
```
# keep select people
plink --vcf new_file.vcf.gz --make-bed --out new_file --keep keep_list.txt

# remove select people
plink --vcf new_file.vcf.gz --make-bed --out new_file --remove remove_list.txt
```

keep_list.txt or remove_list.txt is a text file with the columns (no header, and space delimited): `FID IID`.


Then you can update the sex and affection in the fam file by using a covariate file made from the phenotypic data.

```
plink --vcf new_file.vcf.gz --out new_plink  --keep id_list.txt --pheno pheno_file.txt --pheno-name <pheno_col_name> --update-sex pheno_file.txt --make-bed
```

Covariate file should have the following columns (has a header, space/tab delimited): `FID IID SEX` subsequent columns can be any phenotypes you like. `--update-sex` assumes the 3rd column is sex, but it can be specified (refer to plink documentation).